# Hướng dẫn cài đặt Teleport chi tiết và xử lý lỗi thường gặp

Tài liệu này hướng dẫn mình triển khai Teleport theo mô hình phổ biến cho dự án nội bộ
Một máy Teleport trung tâm chạy Auth và Proxy
Nhiều máy Linux chạy Node để mình SSH thông qua Teleport

Mục tiêu cuối
Mình đăng nhập từ Windows bằng tsh
Mình SSH vào Ubuntu VM qua Teleport
Mình mở rộng thêm các VM nhân viên và vẫn SSH được thông qua Teleport

## 1 Yêu cầu trước khi cài

### 1.1 Máy Teleport trung tâm
Ubuntu Server hoặc Ubuntu Desktop đều được
Khuyến nghị Ubuntu 22.04 LTS
Có IP cố định trong mạng nội bộ
Mở cổng 3080 từ máy Windows tới máy Teleport trung tâm
Mở cổng 3025 và 3022 từ các node Linux tới máy Teleport trung tâm trong mạng nội bộ

### 1.2 Máy node Linux
Mỗi máy Linux cần SSH thông qua Teleport sẽ cài Teleport Node
Các node phải ping được tới máy Teleport trung tâm
Khuyến nghị tạo user Linux dùng để SSH ví dụ ubuntu hoặc teleport

### 1.3 Máy Windows client
Cần tải tsh để login và ssh
Windows PowerShell hoặc Windows Terminal đều dùng được

## 2 Sơ đồ luồng kết nối để hiểu đúng

Windows tsh kết nối tới Teleport Proxy qua HTTPS cổng 3080
Teleport Proxy xác thực và phối hợp với Teleport Auth
Sau đó Teleport Proxy tạo phiên SSH nội bộ tới Teleport Node trên máy Linux cổng 3022

Ghi nhớ quan trọng
Tsh login luôn trỏ tới Proxy cổng 3080
Không bao giờ tsh login vào máy node vì node không chạy proxy

## 3 Cài Teleport trung tâm trên Ubuntu

### 3.1 Cài curl nếu thiếu
Nếu gặp lỗi curl not found

```bash
sudo apt update
sudo apt install curl -y
```

### 3.2 Cài Teleport
Ví dụ cài phiên bản 15

```bash
curl https://goteleport.com/static/install.sh | bash -s 15
```

Kiểm tra

```bash
teleport version
```

### 3.3 Tạo cấu hình Auth Proxy Node trên cùng một máy
Mình dùng cấu hình all in one cho giai đoạn đầu dự án

```bash
sudo teleport configure \
  --roles=node,proxy,auth \
  --cluster-name=lab-teleport \
  --output=/etc/teleport.yaml
```

Mở file cấu hình

```bash
sudo nano /etc/teleport.yaml
```

Đảm bảo các phần sau được bật

```yaml
auth_service:
  enabled: yes

proxy_service:
  enabled: yes
  web_listen_addr: 0.0.0.0:3080

ssh_service:
  enabled: yes
```

### 3.4 Khởi động service
Trên Ubuntu dùng systemd

```bash
sudo systemctl enable teleport
sudo systemctl start teleport
sudo systemctl status teleport
```

### 3.5 Kiểm tra cổng lắng nghe
Mình kiểm tra nhanh cổng 3080 và 3025

```bash
sudo ss -lntp | grep -E '3080|3025|3022'
```

## 4 Tạo user Teleport quản trị và đăng nhập Web

### 4.1 Tạo user admin và gán role mẫu
Ví dụ gán role editor và access và cho phép login Linux user ubuntu

```bash
sudo tctl users add admin --roles=editor,access --logins=ubuntu,teleport
```

Lệnh sẽ in ra link invite
Mình mở link trên Windows trình duyệt
Tạo mật khẩu
Sau đó login vào Web UI

### 4.2 Mở Web UI
Trên Windows mở

https://IP_TELEPORT_TRUNG_TÂM:3080

Nếu cert self signed thì trình duyệt cảnh báo là bình thường trong môi trường nội bộ

## 5 Cài Teleport client tsh trên Windows

### 5.1 Tải CLI Client Tools
Vào trang tải của Teleport
Chọn Windows
Chọn CLI Client Tools
Tải file zip
Giải nén sẽ có tsh.exe

### 5.2 Đặt tsh.exe vào thư mục cố định
Ví dụ

C:\teleport\

### 5.3 Login Teleport từ PowerShell
Mình chạy

```powershell
cd C:\teleport
.\tsh.exe login --proxy=IP_TELEPORT_TRUNG_TÂM:3080 --user=admin --insecure
```

Ghi chú
Tham số insecure chỉ dùng khi mình chưa cài cert đúng chuẩn trong nội bộ
Trong dự án nghiêm túc mình nên dùng cert hợp lệ để bỏ insecure

Kiểm tra trạng thái

```powershell
.\tsh.exe status
```

### 5.4 SSH vào node ngay trên máy trung tâm
Nếu máy trung tâm cũng chạy node
Mình có thể thử

```powershell
.\tsh.exe ssh ubuntu@ten-node
```

Hoặc xem node trước

```powershell
.\tsh.exe ls
```

## 6 Thêm một máy node mới và SSH qua Teleport

### 6.1 Tạo token join node trên máy Teleport trung tâm
Trên Ubuntu Teleport trung tâm

```bash
tctl tokens add --type=node
```

Copy token

### 6.2 Cài Teleport trên máy node Linux
Trên node

```bash
sudo apt update
sudo apt install curl -y
curl https://goteleport.com/static/install.sh | bash -s 15
```

### 6.3 Tạo cấu hình role node và join vào cluster
Trên node

```bash
sudo teleport configure \
  --roles=node \
  --token=TOKEN_VUA_COPY \
  --auth-server=IP_TELEPORT_TRUNG_TÂM:3025 \
  --output=/etc/teleport.yaml
```

Lưu ý
Auth server dùng cổng 3025 trong mạng nội bộ
Không dùng 3080 cho auth server
Không dùng 3022 cho auth server

### 6.4 Gắn label cho node để phục vụ phân quyền
Mở file

```bash
sudo nano /etc/teleport.yaml
```

Sửa phần ssh_service

```yaml
ssh_service:
  enabled: yes
  labels:
    owner: employee-a
```

### 6.5 Khởi động service trên node

```bash
sudo systemctl enable teleport
sudo systemctl start teleport
sudo systemctl status teleport
```

### 6.6 Kiểm tra node đã join chưa từ máy client
Trên Windows

```powershell
.\tsh.exe ls
```

Nếu thấy node mới là OK

### 6.7 SSH vào node mới
Ví dụ user Linux là ubuntu

```powershell
.\tsh.exe ssh ubuntu@ten-node
```

## 7 Phân quyền leader và nhân viên A B theo dự án

Teleport dùng role và label để hạn chế nhìn thấy node và quyền SSH

### 7.1 Gắn label chuẩn cho node A và node B
Ví dụ
Node A labels owner employee-a
Node B labels owner employee-b

### 7.2 Role leader
Leader được SSH cả A và B

```yaml
kind: role
metadata:
  name: leader
spec:
  allow:
    logins: ["ubuntu"]
    node_labels:
      owner: ["employee-a", "employee-b"]
```

### 7.3 Role employee-a

```yaml
kind: role
metadata:
  name: employee-a
spec:
  allow:
    logins: ["ubuntu"]
    node_labels:
      owner: "employee-a"
```

### 7.4 Role employee-b

```yaml
kind: role
metadata:
  name: employee-b
spec:
  allow:
    logins: ["ubuntu"]
    node_labels:
      owner: "employee-b"
```

Tạo role trên máy Teleport trung tâm bằng tctl create
Ví dụ

```bash
tctl create leader-role.yaml
tctl create employee-a-role.yaml
tctl create employee-b-role.yaml
```

Tạo user và gán role

```bash
tctl users add leader --roles=leader --logins=ubuntu
tctl users add employee-a --roles=employee-a --logins=ubuntu
tctl users add employee-b --roles=employee-b --logins=ubuntu
```

Kiểm chứng
Leader tsh ls thấy cả A và B
Employee-a tsh ls chỉ thấy A
Employee-b tsh ls chỉ thấy B

## 8 Lỗi thường gặp và cách xử lý

### 8.1 Lỗi curl not found
Triệu chứng
Chạy curl báo không tìm thấy lệnh

Cách xử lý
Cài curl

```bash
sudo apt update
sudo apt install curl -y
```

### 8.2 Tsh login connection refused tới 3080
Triệu chứng
Get https IP:3080 webapi ping dial tcp connection refused

Nguyên nhân
Proxy không chạy hoặc firewall chặn
Hoặc mình trỏ nhầm IP vào một node không chạy proxy

Cách xử lý
Trên máy Teleport trung tâm kiểm tra service

```bash
sudo systemctl status teleport
sudo ss -lntp | grep 3080
```

Trên Windows đảm bảo login đúng IP Teleport trung tâm
Không login vào IP của node nhân viên

### 8.3 Tsh login báo tls first record does not look like a TLS handshake
Triệu chứng
Get https IP:3022 webapi ping tls first record does not look like a TLS handshake

Nguyên nhân
Mình dùng tsh login vào cổng 3022 hoặc 3025
Các cổng này không phải HTTPS

Cách xử lý
Luôn dùng

```powershell
.\tsh.exe login --proxy=IP_TELEPORT_TRUNG_TÂM:3080 --user=ten-user
```

### 8.4 Tsh ls chỉ thấy 1 node localhost
Triệu chứng
Tsh ls chỉ thấy node máy trung tâm

Nguyên nhân
Node mới chưa join được
Token hết hạn
Auth server IP sai
Network giữa node và auth server chưa thông

Cách xử lý
Trên node kiểm tra service và log

```bash
sudo systemctl status teleport
sudo journalctl -u teleport -n 200
```

Kiểm tra node ping được auth server

```bash
ping IP_TELEPORT_TRUNG_TÂM
```

Kiểm tra auth server port 3025

```bash
nc -zv IP_TELEPORT_TRUNG_TÂM 3025
```

Nếu token hết hạn thì tạo token mới và cấu hình lại

```bash
tctl tokens add --type=node
```

### 8.5 SSH permission denied
Triệu chứng
Tsh ssh báo access denied hoặc permission denied

Nguyên nhân
User Teleport không có quyền login Linux user đó
Role thiếu logins hoặc node_labels

Cách xử lý
Trên client kiểm tra logins hiện có

```powershell
.\tsh.exe status
```

Trên auth server kiểm tra role

```bash
tctl users get ten-user
tctl roles get ten-role
```

### 8.6 Không thấy node vì RBAC
Triệu chứng
Employee-a tsh ls không thấy node B

Đây là đúng thiết kế
Teleport ẩn tài nguyên không có quyền

Nếu leader cũng không thấy thì kiểm tra label node và role leader
Trên node kiểm tra labels trong teleport.yaml
Trên leader role kiểm tra node_labels

### 8.7 Node join nhưng ssh không vào được
Triệu chứng
Tsh ls thấy node nhưng tsh ssh bị lỗi

Nguyên nhân
Thiếu user Linux tương ứng trên node
Hoặc node thiếu ssh_service enabled
Hoặc role thiếu logins

Cách xử lý
Trên node xác nhận user Linux tồn tại

```bash
id ubuntu
```

Trên node xác nhận ssh_service enabled trong /etc/teleport.yaml
Sau đó restart

```bash
sudo systemctl restart teleport
```

### 8.8 Hết hạn chứng chỉ
Triệu chứng
Tsh ssh báo cert expired hoặc cần re login

Cách xử lý
Login lại

```powershell
.\tsh.exe login --proxy=IP_TELEPORT_TRUNG_TÂM:3080 --user=ten-user --insecure
```

## 9 Checklist dự án để mình tự nghiệm thu

1 Teleport trung tâm chạy auth proxy và web 3080 truy cập được
2 Windows tsh login thành công
3 Tsh ls thấy toàn bộ node dự kiến
4 Admin ssh vào từng node thành công
5 Node có label owner đúng theo nhân viên
6 Tạo role leader employee-a employee-b
7 Leader chỉ thấy node A và B và ssh được cả hai
8 Employee-a chỉ thấy node A
9 Employee-b chỉ thấy node B
10 Audit log và session recording kiểm tra được trong Web UI

## 10 Gợi ý nâng cấp khi đưa vào production

1 Cài cert nội bộ hợp lệ để bỏ insecure
2 Tách auth proxy khỏi node trên máy trung tâm khi hệ thống lớn
3 Bật session recording để truy vết
4 Tích hợp SSO và MFA
5 Bật cơ chế request access khi leader cần quyền tạm thời

Hết

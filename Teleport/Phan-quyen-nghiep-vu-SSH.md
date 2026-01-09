# Báo cáo hướng dẫn phân quyền trong Teleport theo luồng nghiệp vụ Leader và Nhân viên A B

Tài liệu này mô tả cách mình thiết kế và triển khai phân quyền truy cập SSH trong Teleport cho mô hình nghiệp vụ

## Mục tiêu nghiệp vụ

- Một tài khoản Leader có thể SSH vào server của hai nhân viên A và B

- Tài khoản Nhân viên A chỉ SSH được vào server A

- Tài khoản Nhân viên B chỉ SSH được vào server B

- Nhân viên A không thể SSH sang server B và ngược lại

- Tài khoản Admin Teleport chỉ dùng để quản trị hệ thống Teleport

## Phạm vi:

- Áp dụng cho Teleport Community hoặc Enterprise

- Tập trung vào SSH Access Control bằng RBAC và Node Labels

- Không triển khai Access Request hay SSO trong tài liệu này

## 1. Tổng quan mô hình quyền của Teleport

- Teleport sử dụng RBAC Role Based Access Control
- Quyền truy cập tài nguyên không dựa theo IP cứng
- Mà dựa trên Role và Labels của tài nguyên

### Trong bài toán SSH
- User Teleport được gán một hoặc nhiều Role
- Role quy định
- Được login Linux user nào logins
- Được nhìn thấy và SSH vào node nào node_labels
- Có được dùng port forwarding hay agent forwarding không
- Có được dùng kubernetes database app hay không nếu bật các service tương ứng

### Khái niệm quan trọng
- Node Label là khóa để Teleport quyết định node nào thuộc phạm vi truy cập của role

## 2. Sơ đồ luồng nghiệp vụ và trách nhiệm

Vai trò chính
Admin Teleport
Leader
Employee A
Employee B

Tài nguyên
Server A
Server B

### Luồng nghiệp vụ chuẩn
- Admin tạo và quản lý Role
- Admin tạo user Leader Employee A Employee B và gán Role tương ứng
- Leader dùng tsh hoặc Web UI để SSH vào Server A và Server B khi cần giám sát hỗ trợ
- Employee A chỉ SSH Server A để làm việc của mình
- Employee B chỉ SSH Server B để làm việc của mình

### Nguyên tắc an toàn
### Nhưng trong bài lab này chúng ta sẽ dùng Admin làm tài khoản leader, và sẽ tạo một Nhanvien1 để test
- Admin không dùng để SSH vận hành hằng ngày
- Leader không có quyền quản trị Teleport
- Employee không thấy tài nguyên ngoài phạm vi

## 3. Thiết kế nhãn node Node Labels

Mục tiêu
Gắn mỗi server vào đúng owner để kiểm soát quyền truy cập

Đề xuất label tối thiểu theo nghiệp vụ
owner nhanvien1 cho Server A

Ví dụ mở rộng khi hệ thống lớn
env dev hoặc prod
team backend hoặc ops
project ten-du-an

Trong tài liệu này mình dùng owner để đơn giản và rõ ràng

## 4. Áp nhãn cho Server A và Server B

Điều kiện
Server A và Server B đã join vào cluster Teleport với vai trò node
Mỗi server có file cấu hình Teleport node tại /etc/teleport.yaml

### 4.1 Gắn label cho Server A
Trên Server A sửa cấu hình

```yaml
ssh_service:
  enabled: yes
  labels:
    owner: employee-a
```

Restart Teleport trên Server A

```bash
sudo systemctl restart teleport
```

### 4.3 Kiểm tra label từ máy client hoặc máy Teleport trung tâm

```bash
tsh ls
```

Kết quả mong muốn
Mình thấy Server A có owner employee-a

Nếu chưa thấy label
Kiểm tra lại file /etc/teleport.yaml và restart teleport service trên node

## 5. Thiết kế Role theo nghiệp vụ

Mình sẽ tạo 2 role chính
- admin
- nhanvien1

Trong tất cả role mình quy định logins
Logins là danh sách user Linux mà Teleport user được phép đăng nhập khi SSH
Ví dụ phổ biến là ubuntu hoặc teleport

Khuyến nghị
Mỗi node Linux cần có sẵn user Linux tương ứng với logins
Ví dụ user admin tồn tại trên Server A và server chính 

## 6. Tạo Role Leader(admin)

Yêu cầu
Leader được SSH vào Server A và Server B
Leader không cần quản trị Teleport

File leader-role.yaml

```yaml
kind: role
metadata:
  name: leader
spec:
  allow:
    logins: ["ubuntu"]
    node_labels:
      owner: ["nhanvien1"]
```

Triển khai role trên Teleport Auth Server

```bash
tctl create leader-role.yaml
```

Giải thích
node_labels giới hạn node theo label owner
Leader sẽ chỉ thấy những node có owner thuộc employee-a hoặc employee-b

## 7. Tạo Role nhanvien1

Yêu cầu
nhanvien1 chỉ SSH được vào Server A
Không thấy Server B

File employee-a-role.yaml

```yaml
kind: role
metadata:
  name: employee-a
spec:
  allow:
    logins: ["ubuntu"]
    node_labels:
      owner: "nhanvien1"
```

Triển khai

```bash
tctl create employee-a-role.yaml
```

## 9. Tạo User Teleport và gán Role

Ghi chú
Các lệnh sau chạy trên Teleport Auth Server
Mỗi lệnh tạo ra một link invite để người dùng đặt mật khẩu

### 9.1 Tạo user Leader

```bash
tctl users add leader --roles=leader --logins=ubuntu
```

### 9.2 Tạo user nhanvien1

```bash
tctl users add nhanvien1 --roles=nhanvien1 --logins=ubuntu
```

## 10. Kiểm chứng quyền theo tiêu chí nghiệp vụ

Mục tiêu của phần này là chứng minh quyền hoạt động đúng
Teleport không chỉ chặn SSH mà còn ẩn node không thuộc quyền

### 10.1 Kiểm chứng Leader thấy cả A và B
Trên Windows hoặc client

```powershell
tsh login --proxy=IP_PROXY:3080 --user=leader --insecure
tsh ls
```

Kỳ vọng
Leader thấy Server A và Server B

Thử SSH

```powershell
tsh ssh ubuntu@server-a
tsh ssh ubuntu@server-b
```

### 10.2 Kiểm chứng Employee A chỉ thấy A
Login

```powershell
tsh login --proxy=IP_PROXY:3080 --user=employee-a --insecure
tsh ls
```

Kỳ vọng
Chỉ thấy Server A
Không thấy Server B

Nếu thử SSH vào B bằng hostname hoặc IP
Teleport phải từ chối

```powershell
tsh ssh ubuntu@server-b
```

### 10.3 Kiểm chứng Employee B chỉ thấy B
Tương tự Employee A

## 11. Bảng đối chiếu yêu cầu nghiệp vụ

Yêu cầu
Leader SSH server A
Đạt

Leader SSH server B
Đạt

Employee A SSH server A
Đạt

Employee A SSH server B
Bị chặn

Employee B SSH server A
Bị chặn

Employee B SSH server B
Đạt

Ngoài ra
Employee A không nhìn thấy Server B trong tsh ls
Employee B không nhìn thấy Server A trong tsh ls

## 12. Lỗi thường gặp khi triển khai phân quyền và cách xử lý

### 12.1 Leader không thấy node nào
Nguyên nhân
Role leader node_labels không khớp label của node
Hoặc node chưa có label

Cách xử lý
Kiểm tra label trên node bằng tsh ls với admin
Kiểm tra role bằng

```bash
tctl roles get leader
```

Đảm bảo owner label đúng employee-a và employee-b

### 12.2 Employee A vẫn thấy Server B
Nguyên nhân
User employee-a đang mang nhiều role hơn dự kiến
Hoặc có role quá rộng được gán nhầm

Cách xử lý
Kiểm tra role của user

```bash
tctl users get employee-a
```

Chỉ nên có role employee-a
Nếu có role khác thì cập nhật lại

```bash
tctl users update employee-a --set-roles=employee-a
```

### 12.3 SSH permission denied dù thấy node
Nguyên nhân
Role thiếu logins tương ứng
Node không có user Linux đó

Cách xử lý
Trên client kiểm tra logins

```bash
tsh status
```

Trên node kiểm tra user Linux

```bash
id ubuntu
```

Nếu thiếu user thì tạo user Linux hoặc đổi logins trong role

### 12.4 Node label đã sửa nhưng tsh ls vẫn không đổi
Nguyên nhân
Teleport node chưa restart
Hoặc mình sửa nhầm file cấu hình

Cách xử lý
Restart teleport trên node

```bash
sudo systemctl restart teleport
```

Xem log nếu cần

```bash
sudo journalctl -u teleport -n 200
```

### 12.5 Nhầm endpoint khi login
Triệu chứng
Login vào IP của node thay vì IP proxy
Báo connection refused hoặc tls handshake

Cách xử lý
Tsh login luôn trỏ tới proxy cổng 3080
Không login vào node

## 13. Checklist nghiệm thu phân quyền

1 Server A có label owner employee-a
2 Server B có label owner employee-b
3 Role leader giới hạn owner employee-a và employee-b
4 Role employee-a chỉ owner employee-a
5 Role employee-b chỉ owner employee-b
6 Leader tsh ls thấy A và B
7 Employee-a tsh ls chỉ thấy A
8 Employee-b tsh ls chỉ thấy B
9 Tất cả SSH test theo nghiệp vụ đạt

## 14. Gợi ý cải tiến khi đưa vào production

1 Tách role leader khỏi quyền quản trị Teleport
2 Bật session recording để leader và admin có thể audit
3 Bật MFA cho leader và admin
4 Dùng access request nếu cần cấp quyền tạm thời
5 Chuẩn hóa label theo env team project để mở rộng

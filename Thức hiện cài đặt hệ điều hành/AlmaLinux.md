# AlmaLinux
## Thực hiện reset password Administrator, root
### Administrator:
#### 1. Setting

Trước tiên, bạn cần đăng nhập vào hệ thống với quyền Administrator. Bạn sẽ làm việc trực tiếp trên máy ảo của mình.
Sau đó truy cập vào Cài đặt -> Hệ thống 
<img width="1280" height="799" alt="image" src="https://github.com/user-attachments/assets/c03cc574-b56a-4193-843b-6d74ebb6b7bb" />

#### 2. Đổi mật khẩu
Chọn Users và đổi mật khẩu Admin
<img width="1270" height="794" alt="image" src="https://github.com/user-attachments/assets/1e56524a-1ef7-4c5f-8ad2-b951e2242afd" />

### Root
### Vào Terminal

<img width="1276" height="798" alt="image" src="https://github.com/user-attachments/assets/52d87dbe-b90e-4449-ad91-671590de9eba" />

## Cấu hình IP
### Bước 1: Mở nmtui
Đầu tiên, mở terminal và chạy lệnh sau để khởi động công cụ nmtui:
```yaml
sudo nmtui
```
### Bước 2: Chỉnh sửa cấu hình mạng

1. Trong giao diện nmtui, chọn Edit a connection và nhấn Enter.

2. Chọn kết nối mạng bạn muốn cấu hình (thường là Wired connection nếu bạn đang sử dụng kết nối Ethernet).

3. Chọn Edit và nhấn Enter.

4. Tìm và chọn mục IPv4 CONFIGURATION.

5. Thay đổi từ Automatic (DHCP) sang Manual.

6. Nhập các thông tin sau:

- Address: Địa chỉ IP tĩnh bạn muốn cấu hình, ví dụ: 192.168.1.100/24.

- Gateway: Địa chỉ Gateway (thường là địa chỉ của router, ví dụ: 192.168.1.1).

- DNS Servers: Địa chỉ của các DNS (bạn có thể sử dụng DNS của Google, ví dụ: 8.8.8.8 và 8.8.4.4).

7. Sau khi nhập các thông tin, chọn OK để lưu thay đổi.

8. Quay lại màn hình chính và chọn Activate a connection để kích hoạt kết nối với các thay đổi mới.

### Bước 3: Kiểm tra kết nối

Sau khi cấu hình xong, bạn có thể kiểm tra lại kết nối mạng bằng lệnh:
```yaml
ip a
```

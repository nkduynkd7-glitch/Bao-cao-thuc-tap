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
### 1. Vào Terminal

<img width="1276" height="798" alt="image" src="https://github.com/user-attachments/assets/52d87dbe-b90e-4449-ad91-671590de9eba" />

## Cấu hình IP


## Gộp/chia mở rộng phân vùng disk

### Chia ổ cứng (Split ổ đĩa)
#### Ví dụ

Bạn có:

Ổ C: 50GB
Muốn chia thành:

C: 30GB

D: 20GB

#### Các bước chia ổ
#### Bước 1: Mở Disk Management

Nhấn Win + X

Chọn Disk Management

#### Bước 2: Thu nhỏ ổ đĩa

Nhấp chuột phải vào ổ C

Chọn Shrink Volume

Nhập dung lượng muốn tách (MB)

50GB = 52000 MB

Nhấn Shrink


<img width="1023" height="796" alt="image" src="https://github.com/user-attachments/assets/0084ca85-af9c-4084-8778-4318e6fcce2c" />
<img width="1017" height="774" alt="image" src="https://github.com/user-attachments/assets/66f06d55-b5d9-45cb-83f5-2f19207a0dc9" />

#### Bước 3: Tạo ổ mới

Vùng trống sẽ hiện là Unallocated

Nhấp chuột phải vào vùng này

Chọn New Simple Volume

Nhấn Next

Gán ký tự ổ (D)

Format NTFS

Finish

<img width="1020" height="767" alt="image" src="https://github.com/user-attachments/assets/95723e33-a742-4619-b210-b6bf0bc2bacc" />
<img width="1020" height="769" alt="image" src="https://github.com/user-attachments/assets/8ff2a532-c3bd-4966-b563-be7820edb4cb" />

#### Kết quả

Bạn có thêm ổ D mới với dung lượng đã chia

<img width="749" height="324" alt="image" src="https://github.com/user-attachments/assets/c926c59d-c4b1-4c19-8187-8ed32d91fd5f" />

### Gộp ổ cứng
#### Ví dụ

Bạn có:

Ổ C: 30GB

Ổ E: 20GB
Muốn gộp E vào C

#### Các bước gộp ổ
#### Bước 1: Mở Disk Management

Nhấn Win + X

Chọn Disk Management

#### Bước 2: Xóa ổ cần gộp

Nhấp chuột phải vào ổ E

Chọn Delete Volume

Ổ E sẽ thành Unallocated

<img width="1015" height="770" alt="image" src="https://github.com/user-attachments/assets/e638f8d7-d86e-4da8-bc84-ba29d8eb93f6" />


#### Bước 3: Gộp vào ổ chính

Nhấp chuột phải vào ổ C

Chọn Extend Volume

Nhấn Next → Next → Finish

<img width="1024" height="766" alt="image" src="https://github.com/user-attachments/assets/3ff93943-2728-4cad-97f8-0f3b616f29c9" />

#### Kết quả

Ổ D sẽ tăng dung lượng (100GB + 50GB = 150GB)
<img width="750" height="333" alt="image" src="https://github.com/user-attachments/assets/2bde5ea6-d9ba-4a86-9eee-da5759d41305" />
















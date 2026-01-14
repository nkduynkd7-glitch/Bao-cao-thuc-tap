# Window Server 2022
## Thực hiện reset password Administrator
### Cách 1:
#### 1. Đăng nhập vào Windows Server 2022

<img width="977" height="502" alt="image" src="https://github.com/user-attachments/assets/91307c51-3d39-433e-9d78-d48e70399dc1" />


#### 2. Mở "Computer Management"

Nhấn Win + X và chọn Computer Management từ menu.

Hoặc, bạn có thể tìm kiếm Computer Management trong menu Start.

####v 3. Chọn "Local Users and Groups"

Trong cửa sổ Computer Management, điều hướng đến System Tools > Local Users and Groups > Users.

#### 4. Chỉnh sửa tài khoản Administrator

Trong mục Users, bạn sẽ thấy danh sách các tài khoản người dùng.

Nhấp chuột phải vào tài khoản Administrator và chọn Set Password.

<img width="978" height="695" alt="image" src="https://github.com/user-attachments/assets/7e2a9109-4be3-4915-b7c9-ad2eff03fc78" />

#### 5. Đặt mật khẩu mới

Một cửa sổ sẽ hiện ra yêu cầu bạn nhập mật khẩu mới cho tài khoản Administrator.

Nhập mật khẩu mới và xác nhận lại. Sau đó nhấn OK.

<img width="1018" height="796" alt="image" src="https://github.com/user-attachments/assets/dc6d9e5a-8ec6-44d9-b7c7-bd6224fd289c" />


#### 6. Đăng xuất và đăng nhập lại

Sau khi đổi mật khẩu, bạn có thể đăng xuất và đăng nhập lại bằng mật khẩu mới.

### Cách 2:
#### 1. Ctrl+Alt+Del
Ở màn hình đăng nhập, ấn tổ hợp Ctrl+Alt+Del sẽ hiện ra các tùy chọn. 

<img width="376" height="406" alt="image" src="https://github.com/user-attachments/assets/7c8d4aee-f235-4fbf-8ad4-e0b0e804ac39" />

Ấn vào Change a pasword

<img width="759" height="711" alt="image" src="https://github.com/user-attachments/assets/980aff63-8ecf-45e8-9bd2-6b7896be834e" />

## Cấu hình IP
### 1. Mở Advanced network settings

Trong phần Network status, cuộn xuống và nhấp vào Advanced network settings.

### 2. Chọn Change adapter options

Trong cửa sổ Advanced network settings, nhấn vào Change adapter options.

<img width="1014" height="794" alt="image" src="https://github.com/user-attachments/assets/6e9c724a-3237-4b56-8749-601594524f01" />


### 3. Mở Properties của adapter mạng

Bạn sẽ thấy danh sách các kết nối mạng (Ethernet, Wi-Fi, v.v.).

Nhấp chuột phải vào kết nối bạn muốn thay đổi IP và chọn Properties.

<img width="1032" height="825" alt="image" src="https://github.com/user-attachments/assets/aad4a21a-890f-4b2a-b1e6-aa824226e537" />


### 4. Cấu hình địa chỉ IP

Trong cửa sổ Properties, chọn Internet Protocol Version 4 (TCP/IPv4) và nhấn Properties.

Chọn Use the following IP address để nhập địa chỉ IP tĩnh.

Nhập địa chỉ IP, Subnet mask, Default gateway và DNS server theo yêu cầu của bạn.

<img width="394" height="448" alt="image" src="https://github.com/user-attachments/assets/23a6ae42-2be9-4917-8d28-018d2a4c5661" />

### 5. Xác nhận và áp dụng

Sau khi nhập xong, nhấn OK để lưu cài đặt và đóng cửa sổ lại.

### 6. Kiểm tra kết nối

Mở Command Prompt và sử dụng lệnh ipconfig để kiểm tra xem địa chỉ IP mới đã được áp dụng đúng chưa.

<img width="1016" height="797" alt="image" src="https://github.com/user-attachments/assets/ac635691-ce2e-4e7e-82b0-63303c01ed75" />

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















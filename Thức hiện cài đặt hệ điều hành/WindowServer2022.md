# Window Server 2022
## Thực hiện reset password Administrator
### Bước 1: Khởi động lại máy WindowServer2022, ấn phím F2
1. Trong màn hình Choose an option
2. Chọn **"Trouble Shoot"**.
3. Sau đó chọn **"Command Prompt"**.
4. <img width="1016" height="791" alt="image" src="https://github.com/user-attachments/assets/8825762e-0094-465a-a681-bc609bf4eb28" />




### Bước 2. Command Prompt
1. Trong Command prompt
 ```bash
    c:
    cd Windows\System32
    ren Utilman.exe Ulilman.exe.old
    copy cmd.exe Utilman.exe
    ```

<img width="676" height="58" alt="image" src="https://github.com/user-attachments/assets/4e1150d4-3fc2-4bdc-b3f3-6778884e64e1" />

### Bước 3. Khởi Động Lại Máy

1. Đóng Command Prompt và chọn **"Continue"** để khởi động lại máy tính.


### Bước 4. Đặt mật khẩu mới

<img width="977" height="502" alt="image" src="https://github.com/user-attachments/assets/91307c51-3d39-433e-9d78-d48e70399dc1" />


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















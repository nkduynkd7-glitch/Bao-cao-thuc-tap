# Ubuntu22.04
## Đổi Password user root và Admin
### Đổi password Admin
#### Các bước reset mật khẩu trên Ubuntu
1. Khởi động lại máy ảo và vào chế độ GRUB:

- Khi máy ảo khởi động lại, nhanh chóng nhấn Shift (hoặc Esc, tùy vào hệ thống) để vào menu GRUB.

2. Chỉnh sửa dòng khởi động GRUB:

- Trong menu GRUB, dùng mũi tên di chuyển xuống Ubuntu (Recovery Mode) (hoặc chế độ khôi phục).

- Chọn Advanced options for Ubuntu nếu bạn không thấy tùy chọn trên, rồi chọn dòng Ubuntu, with Linux [phiên bản] (recovery mode).

<img width="608" height="443" alt="image" src="https://github.com/user-attachments/assets/81e3ae32-bc23-4768-b6c9-435de5c62d1b" />


3. Vào chế độ root (superuser):

- Khi hệ thống vào chế độ recovery, bạn sẽ thấy một menu.

- Di chuyển xuống và chọn root (chế độ root shell prompt).

- Nhấn Enter. Bạn sẽ được đưa vào một shell với quyền root.

<img width="726" height="286" alt="image" src="https://github.com/user-attachments/assets/e6115907-6a7a-4ba3-902c-9f5e3e8059e1" />
  

4. Gắn lại phân vùng hệ thống:

Để có quyền ghi vào hệ thống, gõ lệnh sau để gắn lại hệ thống ở chế độ đọc/ghi:
```yaml
mount -o remount,rw /
```
5. Đặt lại mật khẩu người dùng:

- Sử dụng lệnh passwd để thay đổi mật khẩu cho người dùng của bạn:
```yaml
passwd tên_người_dùng
``` 
<img width="722" height="532" alt="image" src="https://github.com/user-attachments/assets/adf3469f-cbd4-4b18-9ce7-5332c4e4d65d" />

6. Nhập mật khẩu mới:

- Bạn sẽ được yêu cầu nhập mật khẩu mới. Nhập mật khẩu mới và xác nhận lại.
   
<img width="718" height="400" alt="image" src="https://github.com/user-attachments/assets/fa634e99-2e49-4e59-8d37-a431f4eaabc6" />

7. Khởi động lại hệ thống:

Sau khi thay đổi mật khẩu thành công, gõ lệnh sau để khởi động lại máy:
```yaml
reboot
```
 






### Đổi password Root

Thao tác giống như thay đổi với tài khoản Admin
- Ở Bước 5
```yaml
passwd root
```
## Cấu hình IP

1. Mở tệp cấu hình mạng:

Trên Ubuntu 18.04 trở lên, hệ thống mạng sử dụng Netplan để cấu hình. Bạn cần chỉnh sửa tệp cấu hình của Netplan. Mở terminal và chỉnh sửa tệp cấu hình trong thư mục /etc/netplan/.

Đầu tiên, hãy liệt kê các tệp trong thư mục netplan:

```yaml
ls /etc/netplan/
```
Bạn sẽ thấy một tệp có dạng 00-installer-config.yaml hoặc tên tương tự.

2. Chỉnh sửa tệp cấu hình mạng:

Mở tệp cấu hình mạng bằng trình soạn thảo văn bản (ví dụ: nano):
```yaml
sudo nano /etc/netplan/00-installer-config.yaml
```

3. Cấu hình IP tĩnh:

Trong tệp cấu hình này, bạn sẽ thấy phần cấu hình mạng, hãy chỉnh sửa nó để cấu hình IP tĩnh. Một cấu hình ví dụ cho IP tĩnh như sau:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:
      dhcp4: false
      addresses:
        - 192.168.233.1/24  # Thay đổi địa chỉ IP và subnet mask nếu cần
      gateway4: 192.168.233.20  # Địa chỉ gateway
      nameservers:
        addresses:
          - 8.8.8.8   # DNS chính
          - 8.8.4.4   # DNS phụ

```
<img width="306" height="245" alt="image" src="https://github.com/user-attachments/assets/715c95b5-f639-46dd-b278-fe4211407f48" />

4. Lưu và áp dụng cấu hình:

Sau khi chỉnh sửa xong, nhấn Ctrl + X, sau đó nhấn Y và Enter để lưu tệp.

Áp dụng cấu hình bằng lệnh sau:
```yaml
sudo netplan apply
```
<img width="951" height="224" alt="image" src="https://github.com/user-attachments/assets/079db3ac-f1ab-48cb-8cf3-b40384d85cec" />

5. Kiểm tra cấu hình IP:

Để xác nhận rằng IP đã được cấu hình chính xác, bạn có thể sử dụng lệnh:
```yaml
ip a
```
<img width="1032" height="470" alt="image" src="https://github.com/user-attachments/assets/2704291e-bfc9-44cc-b96a-9a2d5b09be22" />



## Gộp/chia mở rộng ở

### Mở rộng ổ

- Trước khi mở rộng ổ

<img width="605" height="289" alt="image" src="https://github.com/user-attachments/assets/74d0ede8-396a-4ff5-aab9-6363cb1177f3" />
  
- Sau khi mở rộng ổ

<img width="600" height="316" alt="image" src="https://github.com/user-attachments/assets/661c6fca-709f-4665-be4c-79a860d56bd9" />
  
- Kết quả

<img width="600" height="290" alt="image" src="https://github.com/user-attachments/assets/a5ae5c17-f901-4c6a-b8f8-b7908cc0fd70" />

### Chia ổ
- Cài GParted

```yaml
sudo apt update
sudo apt install gparted -y
```

<img width="732" height="380" alt="image" src="https://github.com/user-attachments/assets/ebe14e68-b686-495e-8742-94c4606734a4" />

- Mở GParted

```yaml
sudo gparted
```

<img width="772" height="531" alt="image" src="https://github.com/user-attachments/assets/5f362f3d-2f7a-4d06-8dcd-5bc5c72f818a" />

- Ví dụ bạn có 1 phân vùng /dev/sdb đang chiếm hết dung lượng, bạn muốn tách ra thành 2 ổ với mỗi ổ 10GB.

#### Bước 1: Tạo Partition Table (Bắt buộc)
Trước khi chia ổ, bạn phải tạo một bảng điều hướng cho nó:

1. Trên thanh thực đơn của GParted, chọn Device -> Create Partition Table...

2. Chọn loại là gpt (đây là chuẩn hiện đại và ổn định nhất).

<img width="1271" height="792" alt="image" src="https://github.com/user-attachments/assets/273eff7f-6e65-41bc-aa47-b7db567c35a9" />


3. Nhấn Apply. Lúc này ổ đĩa sẽ sẵn sàng để chia.

<img width="765" height="523" alt="image" src="https://github.com/user-attachments/assets/51770a74-6db4-4705-a382-57260b6003d3" />

#### Bước 2: Chia ổ sdb (Ví dụ chia làm 2 ổ)
Giả sử bạn muốn chia thành một ổ 7GB và phần còn lại là ổ khác:

1. Chuột phải vào vùng màu xám (unallocated) -> chọn New.


2. Tại ô New size (MiB), nhập 10240 (tương đương 10GB).
   
<img width="1271" height="792" alt="image" src="https://github.com/user-attachments/assets/a68115a5-2b34-4252-ae86-1b96ad33428b" />

3. Tại ô File system, chọn ext4. Nhấn Add.

4. Lúc này sẽ còn dư một khoảng trống xám. Bạn lại chuột phải vào vùng xám đó -> chọn New.

5. Để mặc định dung lượng còn lại, chọn File system là ext4. Nhấn Add.
<img width="1271" height="792" alt="image" src="https://github.com/user-attachments/assets/a68115a5-2b34-4252-ae86-1b96ad33428b" />


6. Quan trọng: Nhấn biểu tượng dấu tích xanh (V) trên thanh công cụ để thực thi các thay đổi.
<img width="768" height="534" alt="image" src="https://github.com/user-attachments/assets/9a24e904-aa85-4f6c-b4b6-cc20293ff7c0" />

### Bước 3: Cách gộp lại thành một ổ duy nhất
Nếu sau này bạn muốn gộp chúng lại:

1.Chuột phải vào phân vùng thứ 2 (sdb2) -> chọn Delete.

<img width="771" height="534" alt="image" src="https://github.com/user-attachments/assets/89928f7a-275c-4f7e-8ff6-685e4c59d825" />


2.Chuột phải vào phân vùng thứ 1 (sdb1) -> chọn Resize/Move.

3.Dùng chuột kéo thanh dung lượng về phía bên phải cho đến khi lấp đầy khoảng trống. Nhấn Resize/Move.

<img width="766" height="530" alt="image" src="https://github.com/user-attachments/assets/1be830dc-5ce2-48fe-9a62-3c2afd971a7a" />


4.Nhấn dấu tích xanh (V) để áp dụng thay đổi.

<img width="768" height="525" alt="image" src="https://github.com/user-attachments/assets/d667cb8d-653a-436b-91c1-dc57566aa710" />













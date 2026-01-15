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

### Chia ổ

### Bước 1: Xác định ổ cứng của bạn
```yaml
lsblk
```
<img width="589" height="201" alt="image" src="https://github.com/user-attachments/assets/b9fb3963-d17a-4447-975f-cb016faa789e" />

### Bước 2: Sử dụng fdisk để chia ổ cứng

1. Chúng ta sẽ dùng công cụ fdisk để chia ổ cứng.

Chạy lệnh sau để vào fdisk:
```yaml
sudo fdisk /dev/sdb
```
Thay /dev/sdb bằng tên ổ đĩa bạn muốn chia.

<img width="582" height="206" alt="image" src="https://github.com/user-attachments/assets/6fef5053-ebc3-453d-8fa0-602855dbe649" />

2. Nhập m để hiển thị các lệnh có sẵn trong fdisk.

3. Để tạo phân vùng mới, gõ n và chọn kiểu phân vùng (Primary hoặc Extended). Bạn sẽ được yêu cầu nhập các giá trị như số phân vùng và kích thước.

<img width="820" height="418" alt="image" src="https://github.com/user-attachments/assets/5d691976-838a-4d1b-b1f9-b36420e4638b" />


4. Sau khi tạo phân vùng, bạn có thể kiểm tra lại bằng lệnh p.

<img width="566" height="210" alt="image" src="https://github.com/user-attachments/assets/46e422e7-17e3-461c-9700-b0c8b9619a00" />

   

5. Để lưu thay đổi và thoát, gõ w.

### Bước 3: Tạo hệ thống tệp cho phân vùng

1. Sau khi chia ổ cứng, bạn cần tạo hệ thống tệp (filesystem) cho phân vùng mới. Ví dụ, nếu bạn chia ổ thành phân vùng /dev/sda1, bạn có thể tạo filesystem như sau:
```yaml
sudo mkfs.ext4 /dev/sdb1
```
Bạn có thể thay đổi kiểu filesystem nếu cần (ví dụ: ext4, xfs,...).

<img width="626" height="400" alt="image" src="https://github.com/user-attachments/assets/456306e7-2880-4086-8330-8352e3f8eb06" />

2. Mount các phân vùng mới

Sau khi tạo hệ thống tệp, bạn cần gắn (mount) các phân vùng này vào thư mục trong hệ thống.

- Tạo thư mục mount:
Tạo thư mục để gắn các phân vùng. Ví dụ:

<img width="351" height="41" alt="image" src="https://github.com/user-attachments/assets/a528d2cb-59f8-413f-b435-0607c0ba3609" />

- Mount phân vùng vào các thư mục:

Mount phân vùng đầu tiên vào thư mục /mnt/part1:
```yaml
sudo mount /dev/sdb1 /mnt/part1
```
Mount phân vùng thứ hai vào thư mục /mnt/part2:
```yaml
sudo mount /dev/sdb2 /mnt/part2
```
- Đảm bảo phân vùng tự động mount khi khởi động

Để đảm bảo các phân vùng này sẽ tự động được mount khi hệ thống khởi động lại, bạn cần thêm chúng vào tệp /etc/fstab.
Mở tệp /etc/fstab:
```yaml
sudo nano /etc/fstab
```

Thêm các dòng sau vào cuối tệp:
```yaml
/dev/sdb1  /mnt/part1  ext4  defaults  0  2
/dev/sdb2  /mnt/part2  ext4  defaults  0  2
```

<img width="1073" height="307" alt="image" src="https://github.com/user-attachments/assets/07689af5-7641-4315-b4f0-e83a476fda5b" />


Lưu và thoát (Nhấn Ctrl + X, sau đó nhấn Y và Enter).

<img width="619" height="244" alt="image" src="https://github.com/user-attachments/assets/87d75636-2913-47b2-bf57-f08cbf83d313" />




### Gộp ổ

#### Bước 1. Xóa các phân vùng hiện tại

Để gộp lại ổ cứng, bạn cần xóa các phân vùng hiện tại (/dev/sdb1 và /dev/sdb2).

- Chạy lệnh fdisk để vào công cụ quản lý phân vùng:
```yaml
sudo fdisk /dev/sdb
```
- Xóa các phân vùng:

Gõ d để xóa phân vùng.

Hệ thống sẽ yêu cầu bạn chọn số phân vùng muốn xóa. Nếu bạn có hai phân vùng (/dev/sdb1 và /dev/sdb2), bạn sẽ phải thực hiện việc này hai lần.

Sau khi xóa, gõ w để lưu thay đổi.

<img width="705" height="317" alt="image" src="https://github.com/user-attachments/assets/02fe4d88-626b-4ca4-81cb-454a686cb7c4" />

#### Bước 2. Tạo lại phân vùng mới

Sau khi đã xóa các phân vùng cũ, bạn có thể tạo lại một phân vùng duy nhất để sử dụng toàn bộ dung lượng của ổ.

- Tạo một phân vùng mới:

Chạy lại lệnh fdisk:
```yaml
sudo fdisk /dev/sdb
```
1. Gõ n để tạo phân vùng mới.

2. Chọn p (Primary) để tạo phân vùng chính.

3. Chọn số phân vùng, ví dụ 1 cho phân vùng đầu tiên.

4. Nhấn Enter để chọn sector đầu tiên mặc định.

5. Nhấn Enter lần nữa để chọn sector cuối cùng, tức là để phân vùng chiếm toàn bộ dung lượng 20GB.

<img width="747" height="422" alt="image" src="https://github.com/user-attachments/assets/dea839fc-d790-472e-90a2-b00fdd3bda48" />

#### Bước 3. Tạo hệ thống tệp (Filesystem) cho phân vùng mới

Sau khi đã tạo lại phân vùng, bạn cần tạo một hệ thống tệp trên phân vùng mới:
```yaml
sudo mkfs.ext4 /dev/sdb1
```
<img width="596" height="308" alt="image" src="https://github.com/user-attachments/assets/8c101fa4-bd2c-478c-b3c2-8352a6ecdad5" />

#### Bước 4. Tạo thư mục mount
1.Tạo thư mục mount:
```yaml
sudo mkdir /mnt/data
```
2. Mount phân vùng mới vào thư mục:
```yaml
sudo mount /dev/sdb1 /mnt/data
```

#### Bước 5. Cập nhật tệp /etc/fstab

Để phân vùng mới tự động mount khi hệ thống khởi động lại, bạn cần thêm nó vào tệp /etc/fstab.
1. Mở tệp /etc/fstab:
```yaml
sudo nano /etc/fstab
```
2.Thêm dòng sau vào cuối tệp:
```yaml
/dev/sdb1  /mnt/data  ext4  defaults  0  2
```
3. Lưu và thoát (Nhấn Ctrl + X, sau đó nhấn Y và Enter).

#### Bước 6. Kiểm tra lại:
Bạn có thể sử dụng lệnh lsblk hoặc df -h để kiểm tra các phân vùng và điểm mount.
```yaml
lsblk
df -h
```
<img width="577" height="212" alt="image" src="https://github.com/user-attachments/assets/39f362ac-4bb5-49bb-8714-0a5a99274cd1" />









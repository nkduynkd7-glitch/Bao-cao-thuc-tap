# Hướng Dẫn Reset Mật Khẩu Windows Server Sử Dụng Đĩa Cài Đặt

Hướng dẫn này sẽ giúp bạn reset mật khẩu Windows Server bằng cách sử dụng đĩa cài đặt Windows Server.

## Chuẩn Bị

- Đĩa cài đặt Windows Server hoặc USB bootable.
- Máy tính cần reset mật khẩu.

## Bước 1: Khởi Động Từ Đĩa Cài Đặt

1. **Khởi động lại máy tính** Nhấn phím Shift để vào chế độ Recovery
2. Chọn See advanced repair options

![Command Prompt](https://github.com/cuongnvvietis/NhanHoa/blob/main/Docs/Esxi/Picture/Reset%20Password/Screenshot_101.png)
 
3. Chọn Use a device

![Command Prompt](https://github.com/cuongnvvietis/NhanHoa/blob/main/Docs/Esxi/Picture/Reset%20Password/Screenshot_102.png)

4. Chọn khởi động từ CDROM Drive để tiến hành vào windows setup từ đĩa mới

![Command Prompt](https://github.com/cuongnvvietis/NhanHoa/blob/main/Docs/Esxi/Picture/Reset%20Password/Screenshot_103.png)

5. **Nhấn vào liên kết "Repair your computer"** ở góc dưới bên trái của màn hình cài đặt.

![Chọn Repair Your Computer](https://github.com/cuongnvvietis/NhanHoa/blob/main/Docs/Esxi/Picture/Reset%20Password/Screenshot_98.png)

## Bước 2: Sử Dụng Command Prompt

1. Trong màn hình **"Choose an option"**, chọn **"Troubleshoot"**.
2. Chọn **"Advanced options"**.
3. Chọn **"Command Prompt"**.

![Command Prompt](https://github.com/cuongnvvietis/NhanHoa/blob/main/Docs/Esxi/Picture/Reset%20Password/Screenshot_106.png)

4. Chọn **"Command Prompt"**.

![Command Prompt](https://github.com/cuongnvvietis/NhanHoa/blob/main/Docs/Esxi/Picture/Reset%20Password/Screenshot_107.png)

## Bước 3: Thay Đổi File System

1. Sao lưu và thay thế `Utilman.exe`:

    ```bash
    c:
    cd Windows\System32
    ren Utilman.exe Ulilman.exe.old
    copy cmd.exe Utilman.exe
    ```
  ![Command Prompt](https://github.com/cuongnvvietis/NhanHoa/blob/main/Docs/Esxi/Picture/Reset%20Password/Screenshot_100.png)
  
## Bước 4: Khởi Động Lại Máy

1. Đóng Command Prompt và chọn **"Continue"** để khởi động lại máy tính.

## Bước 5: Reset Mật Khẩu

1. Khi máy tính khởi động, ở màn hình đăng nhập, nhấn **Windows Key + U** để mở Command Prompt. Hoặc biểu tượng như hình dưới
2. Trong Command Prompt, gõ lệnh sau để reset mật khẩu:

    ```bash
    net user <username> <newpassword>
    ```

    Thay thế `<username>` bằng tên người dùng và `<newpassword>` bằng mật khẩu mới.
   
 ![Command Prompt](https://github.com/cuongnvvietis/NhanHoa/blob/main/Docs/Esxi/Picture/Reset%20Password/Screenshot_108.png)

4. Đăng nhập bằng mật khẩu mới và khôi phục file `Utilman.exe` nếu cần:

    ```bash
    copy C:\Utilman.exe C:\Windows\System32\Utilman.exe
    ```

## Lưu Ý

- Đảm bảo bạn có quyền truy cập vật lý vào máy tính.
- Thực hiện các bước trên với trách nhiệm và tuân thủ quy định bảo mật.

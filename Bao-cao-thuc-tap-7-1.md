# Báo cáo thực tập ngày 7-1
## 1. Apache Guacamode
### Đã đăng nhập, đổi mật khẩu Guacadmin
<img width="1912" height="935" alt="image" src="https://github.com/user-attachments/assets/5b8763f9-59f9-4301-a0ec-f7ff36fa5062" />
<br><br><br><br>
### Đã SSH thành công vào UbuntuVM
<img width="851" height="457" alt="image" src="https://github.com/user-attachments/assets/9b2d5980-bf0f-48d6-87fd-17725e652664" />

- Vấn đề gặp phải: Không Connect được vào Win11 vì không có SSH server => Sử dụng UbuntuVM 
- Giải pháp: Tạo Adapter Network 2: Host-only sử dụng Ip của Host-only để SSH tới UbuntuVM và thành công.
## 2. Copy-paste bị lỗi khi sử dụng Guacamode SSH 
- Vấn đề gặp phải; khi copy từ máy bên ngoài vào Guacamole thì văn bản sẽ bị dính chữ
<img width="1633" height="178" alt="image" src="https://github.com/user-attachments/assets/88c28627-67d8-4352-a8d6-b0a6190c415f" />
### Giải pháp: Dùng HERE-DOC

- cat << 'EOF' > ten_file.txt
  
   <dán phần text đã copy Ctrl+Shift+V>
  
   EOF
- Ví dụ: Vẫn lấy phần text cũ

  <img width="632" height="393" alt="image" src="https://github.com/user-attachments/assets/27925d01-5b5a-48b1-986f-59c4ed3e463d" />
  
- Kiểm tra lại: cat duy_test.txt

  <img width="544" height="381" alt="image" src="https://github.com/user-attachments/assets/da86caca-36c4-40fc-94fd-626c48ebfc89" />
  <img width="1875" height="937" alt="image" src="https://github.com/user-attachments/assets/e3b9e671-d380-4e86-9c43-23ff8d23ef8c" />






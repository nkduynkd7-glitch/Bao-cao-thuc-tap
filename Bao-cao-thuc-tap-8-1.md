# TELEPORT SSH
## Đã hoàn thành cài đặt và SSH tới UbuntuVM
<img width="1919" height="765" alt="image" src="https://github.com/user-attachments/assets/09bed9c5-ab45-402d-b921-163238034abb" />

## Copy-paste không còn lỗi
<img width="584" height="467" alt="image" src="https://github.com/user-attachments/assets/1c276807-1639-44aa-bf93-5b757cee50be" />

## Upload file bằng Teleport
  - Upload file trên Teleport dạng web:
    <img width="1919" height="939" alt="image" src="https://github.com/user-attachments/assets/d99f4f33-bf31-4a9e-b717-4e955af7e832" />
    
  - Upload file trên Teleport ở PowerShell:
    <img width="957" height="252" alt="image" src="https://github.com/user-attachments/assets/545e06a7-aece-42ed-80c5-8e8bf7e4b00f" />

  - Session Recording: Check được User đã SHH vào server nào, thời gian nào và trong bao lâu
    <img width="1919" height="949" alt="image" src="https://github.com/user-attachments/assets/b10c2c4b-7cc6-4bb7-8385-da77b60bb8da" />

  - Audit Log: Check thông tin chi tiết hành động và thời gian của hành động khi SSH
    <img width="1919" height="881" alt="image" src="https://github.com/user-attachments/assets/5e306370-bed7-4bb2-806e-642fb1389e76" />
    
  - Kiểm tra các File đã upload trong thư mục:
    
    <img width="454" height="122" alt="image" src="https://github.com/user-attachments/assets/ee90d82c-ebe0-4e92-a4ca-6025e382c7bd" />

## Phân quyền trong Teleport
### 1. Cài Teleport và SSH trên VM nhanvien1
  <img width="721" height="414" alt="image" src="https://github.com/user-attachments/assets/ca2358f0-1fbf-493a-a9e0-46f976dae9c2" />
  1.1. TẠO TOKEN JOIN NODE (làm trên Teleport Admin VM)
  <img width="681" height="331" alt="image" src="https://github.com/user-attachments/assets/e047ecbd-b578-4480-8278-ad978b612399" />
  
  1.2.  CẤU HÌNH TELEPORT NODE TRÊN VM A
  sudo teleport configure \
  --roles=node \
  --token=8cd3f978428c26ffce317413960a61ed \
  --auth-server=192.168.233.128:3025 \
  --output=/etc/teleport.yaml
  
  1.3.  GẮN LABEL CHO SERVER NHÂN VIÊN A
  Trên Teleport:
  Mở file sudo nano /etc/teleport.yaml
  Sửa ssh_service:
  ssh_service:
    enabled: yes
    labels:
      owner: nhanvien1

  1.4.  KHỞI ĐỘNG TELEPORT NODE
  <img width="1594" height="530" alt="image" src="https://github.com/user-attachments/assets/0a5268fa-597e-4caa-9fc7-db02eee7ad6f" />

  1.5  KIỂM TRA TỪ TELEPORT ADMIN
  Check xem node đã join chưa
  <img width="904" height="440" alt="image" src="https://github.com/user-attachments/assets/a40ae58d-ddfc-448f-9123-4ecac73aad05" />

  nhanvien1-vitual-machine Đã join




  
  





## 1. Tổng quan về ảo hóa
### 1. Ảo hóa là gì?
Ảo hóa (Virtualization) là công nghệ cho phép tạo ra các phiên bản ảo (thay vì thực thể vật lý) của một cái gì đó, chẳng hạn như hệ điều hành, máy chủ, thiết bị lưu trữ hoặc tài nguyên mạng.

Về cơ bản, ảo hóa cho phép phân chia một máy chủ vật lý thành nhiều "máy ảo" (Virtual Machines - VM) độc lập. Mỗi máy ảo này chạy hệ điều hành riêng và sử dụng một phần tài nguyên (CPU, RAM, ổ cứng) của máy vật lý gốc thông qua một lớp phần mềm gọi là **Hypervisor**.

### 2. Có những loại ảo hóa nào?
Công nghệ ảo hóa rất rộng, nhưng có thể chia thành các loại chính sau:
- **Ảo hóa máy chủ (Server Virtualization):** Phổ biến nhất, chia một máy chủ vật lý thành nhiều máy chủ ảo.
- **Ảo hóa mạng (Network Virtualization):** Mô phỏng các tài nguyên mạng (router, switch, firewall) bằng phần mềm.
- **Ảo hóa lưu trữ (Storage Virtualization):** Tập hợp nhiều thiết bị lưu trữ vật lý thành một bộ lưu trữ duy nhất.
- **Ảo hóa máy để bàn (Desktop Virtualization/VDI):** Chạy hệ điều hành máy tính cá nhân trên máy chủ trung tâm.
- **Ảo hóa ứng dụng (Application Virtualization):** Chạy ứng dụng mà không cần cài đặt trực tiếp lên hệ điều hành máy khách.

### 3. Đặc điểm của từng loại ảo hóa

| Loại ảo hóa | Đặc điểm chính |
| :--- | :--- |
| **Ảo hóa máy chủ** | Tối ưu hóa hiệu suất phần cứng, giảm chi phí mua thiết bị, dễ dàng sao lưu và khôi phục (Snapshot). |
| **Ảo hóa mạng** | Tăng tính linh hoạt, cho phép quản lý mạng bằng mã nguồn (SDN), bảo mật tốt hơn thông qua phân tách mạng. |
| **Ảo hóa lưu trữ** | Giúp quản lý tập trung tài nguyên lưu trữ từ nhiều nguồn khác nhau, dễ dàng mở rộng dung lượng mà không làm gián đoạn hệ thống. |
| **Ảo hóa máy để bàn** | Giúp nhân viên làm việc từ xa dễ dàng, dữ liệu được bảo mật tập trung tại máy chủ thay vì nằm trên máy cá nhân. |
| **Ảo hóa ứng dụng** | Giảm xung đột giữa các phần mềm, dễ dàng cập nhật ứng dụng hàng loạt từ máy chủ. |

## 2. Ảo hóa Promox
### 1.Cài đặt
Đã hoàn thành cài đặt

<img width="1919" height="989" alt="image" src="https://github.com/user-attachments/assets/d5b687ab-356f-4a2b-aa05-a0dc2fc7b0af" />

### 2.Add Storage
- Thêm ổ /dev/sdb/ -> WipeDisk -> Initialize GPT
<img width="1602" height="695" alt="image" src="https://github.com/user-attachments/assets/8085643a-fdab-4f08-93f5-da3e288130c7" />
- Create Directory
<img width="1599" height="689" alt="image" src="https://github.com/user-attachments/assets/8cc29ad3-15e7-4522-ace7-089180ea567f" />
- Đã tạo xong
<img width="1607" height="696" alt="image" src="https://github.com/user-attachments/assets/d000b183-ced1-4ede-88fd-49b84a2c542f" />

### 3.Cấu hình network dạng Flat, Multi VLAN
- Flat network: Tất cả VM cùng chung 1 mạng, không VLAN.
  + Kiểm tra trạng thái Bridge trên giao diện web
  <img width="1602" height="689" alt="image" src="https://github.com/user-attachments/assets/32f97ef1-2732-43c0-a586-bb43fe4e37e2" />
  + Cấu hình DNS để máy ảo ra được Internet
  <img width="1601" height="693" alt="image" src="https://github.com/user-attachments/assets/b6c32a87-fe54-4b2e-828c-bfcfa2fc8945" />
  + Tạo máy ảo để test Flat Network

### 4. Tạo máy ảo và gộp ổ cứng giữa các dât store
- Do dung lượng của ổ cứng không đủ để upload file ISO nên phải gộp ổ local-lvm vào local (pve)
- Các bước để gộp ổ:
  + Xóa định nghĩa storage trong cấu hình Proxmox: pvesm free local-lvm
  + Xóa phân vùng LVM thực tế: lvremove /dev/pve/data
  + Mở rộng phân vùng root (local) để lấy hết dung lượng trống: lvextend -l +100%FREE /dev/pve/root
  + Cập nhật hệ thống tệp (File System) để không cần khỏi động lại: resize2fs /dev/mapper/pve-root
- Kết quả:
  <img width="1919" height="669" alt="image" src="https://github.com/user-attachments/assets/01cc9c65-3415-4a87-92b9-2e6ec33740c3" />
-Cài đặt thành công Ubuntu
<img width="798" height="493" alt="image" src="https://github.com/user-attachments/assets/ce9573a2-efdc-406f-b485-3bcefb35e3b9" />
- Tạo máy ảo: Ubuntu-test
<img width="1919" height="777" alt="image" src="https://github.com/user-attachments/assets/b32a4954-d5ba-4ba1-96ea-eb5b3974d8e1" />

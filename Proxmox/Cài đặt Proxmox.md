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

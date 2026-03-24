
# BÁO CÁO CÀI ĐẶT VÀ CẤU HÌNH OPENCLAW TRÊN PROXMOX

## 1. Tổng quan
OpenClaw là hệ thống AI gateway cho phép tích hợp nhiều model AI và kênh giao tiếp. Mục tiêu:
- Tự host AI
- Kết nối model (OpenAI/OpenRouter)
- Cung cấp dashboard quản trị
- Mở rộng chatbot

---

## 2. Môi trường triển khai
### 2.1 Hạ tầng
- Proxmox VE
- VM Ubuntu
- Network: Bridge DMZ VLAN20

### 2.2 Máy ảo
- Ubuntu Server 25.04 (64-bit)
- CPU: 2-8 vCPU
- RAM: ≥4GB
- Disk: ≥30GB
- Network: VirtIO

---

## 3. Cài đặt hệ điều hành
ISO: ubuntu-25.04-live-server-amd64.iso

---

## 4. Cài đặt OpenClaw

### 4.1 Cài Node.js
```bash
sudo apt update
sudo apt install -y curl build-essential
curl -fsSL https://deb.nodesource.com/setup_24.x | sudo bash -
sudo apt install -y nodejs
```

### 4.2 Cài OpenClaw
```bash
sudo npm install -g openclaw@latest
```

---

## 5. Onboarding

```bash
openclaw onboard --install-daemon
```

### Lựa chọn
- Security: Yes
- Mode: QuickStart
- Provider: OpenAI
- Auth: API key
- Model: openrouter/free
- Skip plugins & hooks

---

## 6. Chạy hệ thống

```bash
openclaw gateway run
```

---

## 7. SSH Tunnel

```bash
ssh -N -L 18889:127.0.0.1:18789 openclaw@IP_VM
```

Truy cập:
http://127.0.0.1:18889

---

## 8. Dashboard

```bash
openclaw dashboard --no-open
```

---

## 9. Lỗi & xử lý

### SSH lỗi
```bash
sudo apt install openssh-server
```

### Tunnel lỗi
→ chạy trên Windows, đổi port

### device signature expired
- Xóa cache
- Dùng tab ẩn danh
- Đồng bộ thời gian

---

## 10. Tạo Gateway Token

```bash
openssl rand -hex 32
```

```bash
openclaw config set gateway.auth.token "TOKEN"
```

---

## 11. Kiểm tra

```bash
openclaw gateway status
openclaw logs --follow
openclaw doctor
```

---

## 12. Sử dụng

### Dashboard
Browser + SSH tunnel

### Terminal
```bash
openclaw tui
```

---

## 13. Kết luận
- Cài đặt thành công
- Gateway hoạt động ổn định
- Có thể truy cập dashboard
- Có thể dùng TUI

---

## 14. Khuyến nghị
- Dùng HTTPS reverse proxy
- Đổi token định kỳ
- Backup ~/.openclaw

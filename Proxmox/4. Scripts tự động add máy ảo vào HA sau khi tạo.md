# PHẦN A – CHUẨN BỊ
## 1. Cài jq (bắt buộc để parse API)
```yaml
apt update
apt install -y jq
```
jq --version

## 2. Test API lấy VM toàn cluster (rất quan trọng)
```
pvesh get /cluster/resources --type vm --output-format json | jq -r '.[].vmid'
```
Nếu bước này OK → đi tiếp.

# PHẦN B – SCRIPT ADD VM VÀO HA
## 3. Tạo thư mục script
```
mkdir -p /root/scripts
```
## 4. Tạo file script
```
nano /root/scripts/add_vm_to_ha.sh
```
## 5. Cấp quyền chạy
```
chmod +x /root/scripts/add_vm_to_ha.sh
```
## 6. Test chạy tay (rất nên làm 1 lần)
```
/root/scripts/add_vm_to_ha.sh
```
Sau đó kiểm tra:
```
ha-manager status
```
# PHẦN C – SYSTEMD TIMER (02:00 SÁNG)
## 7. Tạo service
```
nano /etc/systemd/system/add-vm-to-ha.service
```

Dán:
```
[Unit]
Description=Add VM to Proxmox HA

[Service]
Type=oneshot
ExecStart=/root/scripts/add_vm_to_ha.sh
```
## 8. Tạo timer (02:00 mỗi ngày)
```
nano /etc/systemd/system/add-vm-to-ha.timer
```

Dán:
```
[Unit]
Description=Run add VM to HA daily at 02:00

[Timer]
OnCalendar=*-*-* 02:00:00
Persistent=true

[Install]
WantedBy=timers.target
```
## 9. Enable timer
```
systemctl daemon-reload
systemctl enable --now add-vm-to-ha.timer
```
🔍 Kiểm tra timer
```
systemctl list-timers | grep add-vm-to-ha
```
🧪 Test timer ngay (không đợi 2h sáng)
```
systemctl start add-vm-to-ha.service
```

Xem log:
```
journalctl -u add-vm-to-ha.service
```

Scripts trong file "/root/scripts/add_vm_to_ha.sh"


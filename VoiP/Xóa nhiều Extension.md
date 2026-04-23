# Báo Cáo: Xóa Hàng Loạt Extension trên Hệ Thống HAPBX

---

## Thông Tin Chung

| Mục | Chi tiết |
|-----|----------|
| **Hệ thống** | HAPBX 4.5.1-6 |
| **Server** | tpl-hapbx-us-01 |
| **OS** | Debian GNU/Linux 12 (Bookworm) |
| **Asterisk** | 20.15.0 |
| **Database** | MariaDB 10.11.14 |
| **Ngày thực hiện** | 2026-04-23 |
| **Người thực hiện** | Administrator |

---

## 1. Mục Tiêu

Xóa 5 extension không cần thiết (106 → 110) khỏi hệ thống HAPBX mà **không phải xóa từng cái một** trên giao diện web.

### Danh sách extension cần xóa:

| Extension | Tên | Extension ID |
|-----------|-----|-------------|
| 106 | Logiclab Ba Trieu 106 | 6 |
| 107 | Logiclab Ba Trieu 107 | 7 |
| 108 | Logiclab Ba Trieu 108 | 8 |
| 109 | Logiclab Ba Trieu 109 | 9 |
| 110 | Logiclab Ba Trieu 110 | 10 |

---

## 2. Khảo Sát Giao Diện Web

Trước tiên, đã kiểm tra các tính năng có sẵn trên giao diện web HAPBX:

| Tính năng | Chức năng | Hỗ trợ xóa hàng loạt? |
|-----------|-----------|----------------------|
| **Bulk Extensions** | Tạo nhiều extension cùng lúc | ❌ Không |
| **Bulk Modification** | Chỉnh sửa nhiều extension cùng lúc | ❌ Không |
| **Import Extensions** | Import từ file CSV | ❌ Không |
| **Export Extensions** | Export ra file CSV | ❌ Không |

> **Kết luận:** HAPBX **không hỗ trợ xóa hàng loạt extension** trên giao diện web. Bắt buộc phải thao tác trực tiếp qua SSH và database.

---

## 3. Phương Án Thực Hiện

Thực hiện xóa trực tiếp qua **SSH + MariaDB**, theo các bước:

1. Backup toàn bộ database
2. Xác định database và bảng chứa extension
3. Kiểm tra dữ liệu trước khi xóa
4. Xóa dữ liệu theo đúng thứ tự (bảng phụ → bảng chính)
5. Reload Asterisk

---

## 4. Các Bước Thực Hiện Chi Tiết

### Bước 1: Backup Database

```bash
mysqldump -u root -p --all-databases > backup_20260423.sql
```

> ✅ Luôn backup trước khi thao tác với database để có thể khôi phục nếu xảy ra lỗi.

---

### Bước 2: Đăng Nhập MariaDB

```bash
mysql -u root -p
```

---

### Bước 3: Xác Định Database

```sql
SHOW DATABASES;
```

**Kết quả:**

```
+--------------------+
| Database           |
+--------------------+
| astboard           |
| asterisk           |
| information_schema |
| mysql              |
| ombutel            |
| performance_schema |
| phpmyadmin         |
| provisioning       |
| sonata_billing     |
| sys                |
| telerec            |
| vitalpbx_logs      |
+--------------------+
```

---

### Bước 4: Tìm Bảng Chứa Extensions

```sql
SELECT table_schema, table_name 
FROM information_schema.tables 
WHERE table_name LIKE '%extension%' 
   OR table_name LIKE '%device%' 
   OR table_name LIKE '%user%';
```

**Kết quả quan trọng:** Database **`ombutel`** chứa các bảng liên quan đến extension:

| Bảng | Mô tả |
|------|-------|
| `ombu_extensions` | Bảng chính chứa thông tin extension |
| `ombu_extensions_vm` | Voicemail của extension |
| `ombu_extensions_contact_info` | Thông tin liên lạc |
| `ombu_extension_diversions` | Chuyển hướng cuộc gọi |
| `ombu_extension_pea` | Cấu hình PEA |
| `ombu_pjsip_devices` | Thiết bị PJSIP |
| `ombu_sip_devices` | Thiết bị SIP |
| `ombu_iax_devices` | Thiết bị IAX |
| `ombu_virtual_devices` | Thiết bị Virtual |
| `ombu_devices` | Bảng thiết bị tổng hợp |

---

### Bước 5: Kiểm Tra Dữ Liệu Trước Khi Xóa

```sql
USE ombutel;
SELECT extension_id, extension, name 
FROM ombu_extensions 
WHERE extension BETWEEN 106 AND 110;
```

**Kết quả xác nhận:**

```
+--------------+-----------+-----------------------+
| extension_id | extension | name                  |
+--------------+-----------+-----------------------+
|            6 | 106       | Logiclab Ba Trieu 106 |
|            7 | 107       | Logiclab Ba Trieu 107 |
|            8 | 108       | Logiclab Ba Trieu 108 |
|            9 | 109       | Logiclab Ba Trieu 109 |
|           10 | 110       | Logiclab Ba Trieu 110 |
+--------------+-----------+-----------------------+
```

> ✅ Xác nhận đúng 5 extension cần xóa với `extension_id` từ 6 đến 10.

---

### Bước 6: Xóa Dữ Liệu

> ⚠️ **Quan trọng:** Phải xóa các bảng phụ (foreign key) **trước**, sau đó mới xóa bảng chính `ombu_extensions`.

```sql
-- Xóa các bảng phụ trước
DELETE FROM ombu_extensions_vm WHERE extension_id BETWEEN 6 AND 10;
DELETE FROM ombu_extensions_contact_info WHERE extension_id BETWEEN 6 AND 10;
DELETE FROM ombu_extension_diversions WHERE extension_id BETWEEN 6 AND 10;
DELETE FROM ombu_extension_pea WHERE extension_id BETWEEN 6 AND 10;
DELETE FROM ombu_pjsip_devices WHERE extension_id BETWEEN 6 AND 10;
DELETE FROM ombu_sip_devices WHERE extension_id BETWEEN 6 AND 10;
DELETE FROM ombu_iax_devices WHERE extension_id BETWEEN 6 AND 10;
DELETE FROM ombu_virtual_devices WHERE extension_id BETWEEN 6 AND 10;
DELETE FROM ombu_devices WHERE extension_id BETWEEN 6 AND 10;

-- Xóa bảng chính
DELETE FROM ombu_extensions WHERE extension_id BETWEEN 6 AND 10;
```

---

### Bước 7: Thoát MySQL và Reload Asterisk

```bash
exit
asterisk -rx "core reload"
```

---

## 5. Lưu Ý Quan Trọng

| Lưu ý | Chi tiết |
|-------|----------|
| **Dùng extension_id** | Dùng `extension_id` (6-10) làm điều kiện lọc, không phải số extension (106-110) vì đó là khóa chính |
| **Thứ tự xóa** | Xóa bảng phụ trước, bảng chính sau để tránh lỗi foreign key constraint |
| **Backup bắt buộc** | Luôn backup toàn bộ database trước khi xóa |
| **Reload Asterisk** | Bắt buộc reload sau khi xóa để hệ thống cập nhật cấu hình |
| **Không có UI** | HAPBX không hỗ trợ xóa hàng loạt trên giao diện web — đây là hạn chế của hệ thống |

---

## 6. Tổng Kết

- ✅ Xác định được hệ thống không hỗ trợ xóa hàng loạt qua giao diện web
- ✅ Backup database thành công trước khi thao tác
- ✅ Xác định đúng database (`ombutel`) và bảng chứa extension
- ✅ Kiểm tra và xác nhận dữ liệu trước khi xóa
- ✅ Xóa đúng 5 extension (106-110) theo đúng thứ tự
- ✅ Reload Asterisk để áp dụng thay đổi

---

*Báo cáo được tạo ngày 2026-04-23 | Hệ thống HAPBX tpl-hapbx-us-01*

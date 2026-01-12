# BÁO CÁO TRIỂN KHAI PHÂN QUYỀN SSH VỚI TELEPORT

## 1. Tổng quan dự án

Dự án sử dụng Teleport để quản lý truy cập SSH theo mô hình Zero Trust, thay thế cách SSH truyền thống bằng user và SSH key tĩnh.  
Mục tiêu của dự án là quản lý tập trung quyền truy cập, phân quyền rõ ràng giữa Admin và Nhân viên, đồng thời kiểm soát truy cập dựa trên nhãn (label-based access control).

Hệ thống được triển khai trên nhiều máy ảo Ubuntu chạy trong môi trường VMware.

---

## 2. Phân mọi quyền cho tài khoản admin

### 2.1 Mục tiêu
Tài khoản admin có toàn quyền:
- Xem tất cả server trong cluster
- SSH vào tất cả máy ảo
- Quản lý user, role và node của Teleport

### 2.2 Cấu hình role admin

```yaml
kind: role
version: v7
metadata:
  name: role-teleport-admin
spec:
  allow:
    logins:
      - teleport
    node_labels:
      "*": "*"
```

### 2.3 Gán role cho admin

```bash
sudo tctl users update admin --set-roles=role-teleport-admin
```

### 2.4 Kết quả
- Admin đăng nhập thành công bằng tsh login
- Thấy toàn bộ máy ảo trên Teleport Web UI
- SSH được vào tất cả server

---

## 3. Tạo máy ảo nhanvien1-virtual-machine và phân quyền cho nhanvien1

### 3.1 Mục tiêu
- Server nhanvien1 chỉ dành cho user nhanvien1
- Không cho phép truy cập sang server khác

### 3.2 Cấu hình Teleport trên nhanvien1-virtual-machine

```yaml
teleport:
  nodename: nhanvien1-virtual-machine
  auth_servers:
    - 192.168.233.128:3025

auth_service:
  enabled: "no"

ssh_service:
  enabled: "yes"
  labels:
    owner: nhanvien1

proxy_service:
  enabled: "no"
```

### 3.3 Tạo role cho nhanvien1

```yaml
kind: role
version: v7
metadata:
  name: role-employee-nhanvien1
spec:
  allow:
    logins:
      - teleport
    node_labels:
      owner: nhanvien1
```

### 3.4 Gán role cho user nhanvien1

```bash
sudo tctl users update nhanvien1 --set-roles=role-employee-nhanvien1
```

### 3.5 Kết quả
- nhanvien1 chỉ nhìn thấy server của mình
- SSH thành công vào nhanvien1-virtual-machine
- Không truy cập được server khác

---

## 4. Tạo máy ảo nhanvien2-virtual-machine và phân quyền cho nhanvien2

### 4.1 Mục tiêu
- Server nhanvien2 chỉ dành cho user nhanvien2

### 4.2 Cấu hình Teleport trên nhanvien2-virtual-machine

```yaml
teleport:
  nodename: nhanvien2-virtual-machine
  auth_servers:
    - 192.168.233.128:3025

auth_service:
  enabled: "no"

ssh_service:
  enabled: "yes"
  labels:
    owner: nhanvien2

proxy_service:
  enabled: "no"
```

### 4.3 Tạo role cho nhanvien2

```yaml
kind: role
version: v7
metadata:
  name: role-employee-nhanvien2
spec:
  allow:
    logins:
      - teleport
    node_labels:
      owner: nhanvien2
```

### 4.4 Gán role cho user nhanvien2

```bash
sudo tctl users update nhanvien2 --set-roles=role-employee-nhanvien2
```

### 4.5 Kết quả
- nhanvien2 chỉ thấy server nhanvien2-virtual-machine
- SSH thành công vào server của mình

---

## 5. Phân quyền cho nhanvien1 SSH được vào server nhanvien2

### 5.1 Mục tiêu
- Cho phép nhanvien1 truy cập thêm server nhanvien2
- Không ảnh hưởng quyền của nhanvien2

### 5.2 Cập nhật role nhanvien1

```yaml
kind: role
version: v7
metadata:
  name: role-employee-nhanvien1
spec:
  allow:
    logins:
      - teleport
    node_labels:
      owner:
        - nhanvien1
        - nhanvien2
```

Áp dụng cấu hình:

```bash
sudo tctl create -f role-employee-nhanvien1.yaml
```

### 5.3 Kết quả
- nhanvien1 thấy được 2 server
- SSH được vào cả hai server
- nhanvien2 vẫn chỉ truy cập server của mình

---

## 6. Kết luận

Hệ thống Teleport đã được triển khai thành công với:
- Phân quyền rõ ràng theo vai trò
- Kiểm soát truy cập dựa trên label
- Không sử dụng SSH key tĩnh
- Quản lý tập trung, dễ mở rộng

---

## 7. Bảng phân quyền tổng thể

| User | Server được SSH |
|-----|----------------|
| admin | Tất cả server |
| nhanvien1 | nhanvien1, nhanvien2 |
| nhanvien2 | nhanvien2 |

---

Báo cáo kết thúc.

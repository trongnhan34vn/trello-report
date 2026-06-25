# Phân tích nghiệp vụ - Use Cases hệ thống Nello

## Actor

| Actor | Mô tả |
|---|---|
| **Người dùng (User)** | Actor duy nhất của hệ thống. Bao gồm toàn bộ người tương tác với hệ thống — từ chưa đăng nhập đến đã đăng nhập. Quyền thực thi từng use case phụ thuộc vào trạng thái xác thực và role trong Workspace/Board tương ứng (ADMIN, MEMBER, VIEWER). |

---

## Use Case tổng thể và thành phần

### UC-01: Quản lý xác thực

| UC-ID | Use Case thành phần |
|---|---|
| UC-01.1 | Đăng ký tài khoản |
| UC-01.2 | Xác nhận tài khoản bằng OTP |
| UC-01.3 | Gửi lại mã OTP |
| UC-01.4 | Đăng nhập |
| UC-01.5 | Làm mới access token |

---

### UC-02: Quản lý người dùng

| UC-ID | Use Case thành phần |
|---|---|
| UC-02.1 | Xem thông tin cá nhân |
| UC-02.2 | Chỉnh sửa thông tin cá nhân |

---

### UC-03: Quản lý Workspace

| UC-ID | Use Case thành phần |
|---|---|
| UC-03.1 | Tạo Workspace |
| UC-03.2 | Xem danh sách Workspace |
| UC-03.3 | Xem chi tiết Workspace |
| UC-03.4 | Mời thành viên vào Workspace |
| UC-03.5 | Xem danh sách thành viên Workspace |

---

### UC-04: Quản lý Board

| UC-ID | Use Case thành phần |
|---|---|
| UC-04.1 | Tạo Board |
| UC-04.2 | Xem chi tiết Board |
| UC-04.3 | Cập nhật tên Board |
| UC-04.4 | Xóa Board |
| UC-04.5 | Thêm thành viên vào Board |

---

### UC-05: Quản lý List

| UC-ID | Use Case thành phần |
|---|---|
| UC-05.1 | Tạo List |
| UC-05.2 | Đổi tên List |
| UC-05.3 | Xóa List |
| UC-05.4 | Sắp xếp thứ tự List |

---

### UC-06: Quản lý Card

| UC-ID | Use Case thành phần |
|---|---|
| UC-06.1 | Tạo Card |
| UC-06.2 | Xem chi tiết Card |
| UC-06.3 | Cập nhật Card |
| UC-06.4 | Đánh dấu Card hoàn thành |
| UC-06.5 | Di chuyển Card giữa các List |
| UC-06.6 | Xóa Card |

---

### UC-07: Quản lý Checklist

| UC-ID | Use Case thành phần |
|---|---|
| UC-07.1 | Tạo Checklist |
| UC-07.2 | Cập nhật Checklist |
| UC-07.3 | Xóa Checklist |
| UC-07.4 | Tạo Checklist Item |
| UC-07.5 | Cập nhật Checklist Item |
| UC-07.6 | Đánh dấu Checklist Item hoàn thành |
| UC-07.7 | Xóa Checklist Item |

---

### UC-08: Quản lý thành viên Card

| UC-ID | Use Case thành phần |
|---|---|
| UC-08.1 | Gán thành viên vào Card |
| UC-08.2 | Hủy gán thành viên khỏi Card |

---

## Tổng kết

| Mục | Số lượng |
|---|---|
| Actor | 1 |
| Use case tổng thể | 8 |
| Use case thành phần | 36 |

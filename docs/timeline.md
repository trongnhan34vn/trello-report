# Timeline phát triển

## Tổng quan tiến độ

| Tuần | Thời gian | Giai đoạn | Nội dung chính |
|---|---|---|---|
| W1 | 30/03 – 05/04/2026 | Khởi tạo | Setup dự án, nghiên cứu công nghệ |
| W2 | 06/04 – 12/04/2026 | Thiết kế | API design (Swagger), Database schema |
| W3 | 13/04 – 19/04/2026 | Backend – Auth | AWS Cognito, Authentication APIs |
| W4 | 20/04 – 26/04/2026 | Backend – Core | Workspace, Board, List APIs |
| W5 | 27/04 – 03/05/2026 | Backend – Core | Card, Checklist, ChecklistItem APIs |
| W6 | 04/05 – 10/05/2026 | Frontend – Auth & Dashboard | Auth UI, Dashboard, Workspace/Board UI |
| W7 | 11/05 – 17/05/2026 | Frontend – Board | Workspace Detail, Kanban Board, Drag-and-Drop |
| W8 | 18/05 – 24/05/2026 | Frontend – Hoàn thiện | Board Member, Card Detail, System Testing |

---

## Chi tiết theo tuần

### Tuần 1 — Khởi tạo (30/03 – 05/04/2026)
**Giai đoạn:** Setup & Research

**Đã thực hiện:**
- Khởi tạo Spring Boot project, cấu hình PostgreSQL
- Thiết lập AWS RDS (PostgreSQL instance)
- Tích hợp Flyway migration
- Thiết lập Swagger / OpenAPI
- Nghiên cứu: Spring Boot, PostgreSQL, AWS Cognito, AWS RDS, RESTful API

**Kết quả:** Dự án khởi động, kết nối RDS hoạt động, Swagger UI chạy được

---

### Tuần 2 — Thiết kế (06/04 – 12/04/2026)
**Giai đoạn:** API & DB Design

**Đã thực hiện:**
- Thiết kế toàn bộ API documentation trên Swagger (14 nhóm endpoint)
- Thiết kế Database Schema hoàn chỉnh → V1__init.sql
- Nghiên cứu kiến trúc AWS Cognito (User Pool, JWT)
- Nghiên cứu AWS RDS (tính năng, bảo mật)
- Xác định kiến trúc RESTful API và lý do lựa chọn

**Kết quả:** Swagger documentation đầy đủ, schema database sẵn sàng migrate

---

### Tuần 3 — Backend Authentication (13/04 – 19/04/2026)
**Giai đoạn:** Backend – Module Auth

**Đã thực hiện:**
- Tạo AWS Cognito User Pool, cấu hình App Client, chính sách mật khẩu
- Tích hợp AWS SDK (`CognitoIdentityProviderClient`) vào Spring Boot
- Triển khai `CognitoConfig.java`, `CognitoService.java`, `AuthService.java`
- 5 API Auth: `/sign-up`, `/confirm-sign-up`, `/sign-in`, `/refresh-token`, `/resend-code`
- Lưu token vào HttpOnly Cookie
- Kiểm thử 14 test cases (TC01–TC14)

**Kết quả:** Luồng đăng ký và đăng nhập hoạt động ổn định

---

### Tuần 4 — Backend Core (20/04 – 26/04/2026)
**Giai đoạn:** Backend – Workspace, Board, List

**Đã thực hiện:**
- API Workspace: tạo, lấy danh sách (TC01–TC05)
- API Board: tạo, cập nhật, xóa, lấy danh sách (TC06–TC12)
- API List: tạo, cập nhật, lấy danh sách theo board (TC13–TC16)
- Cơ chế xác thực token trong từng request
- Xử lý phân quyền (403, 404)

**Kết quả:** Hoàn thành 6/9 nhóm API cốt lõi

---

### Tuần 5 — Backend Core (27/04 – 03/05/2026)
**Giai đoạn:** Backend – Card, Checklist

**Đã thực hiện:**
- API Card: CRUD đầy đủ + position cho drag-and-drop (TC17–TC22)
- API Checklist: tạo, cập nhật, xóa, lấy theo card (TC23)
- API ChecklistItem: tạo, cập nhật trạng thái, xóa (TC24–TC25)
- Logic di chuyển Card giữa List (drag-and-drop)
- Chuẩn hóa toàn bộ hệ thống API (format, validation, edge cases)

**Kết quả:** Backend hoàn chỉnh, sẵn sàng cho Frontend

---

### Tuần 6 — Frontend Auth & Dashboard (04/05 – 10/05/2026)
**Giai đoạn:** Frontend – Authentication & Dashboard

**Đã thực hiện:**
- Màn hình Sign Up (`/sign-up`): form đăng ký, validation, tích hợp API
- Màn hình Confirm Sign Up (`/confirm-sign-up`): OTP, resend code
- Màn hình Sign In (`/sign-in`): form đăng nhập, redirect Dashboard
- Dashboard: danh sách Workspace + Board, empty state
- Xử lý session: auto refresh token, redirect nếu session hết hạn
- Tạo Workspace UI (form + category dropdown)
- Tạo Board UI (chọn background + workspace)
- Kiểm thử tích hợp TC01–TC09

**Kết quả:** Luồng người dùng từ đăng ký đến Dashboard hoàn chỉnh

---

### Tuần 7 — Frontend Board (11/05 – 17/05/2026)
**Giai đoạn:** Frontend – Workspace Detail, Kanban, Drag-and-Drop

**Đã thực hiện:**
- Workspace Detail (`/workspaces/:id`): thông tin, board list, member list
- Mời Member vào Workspace (modal + API)
- Board Kanban (`/boards/:id`): hiển thị List columns + Cards
- CRUD List trực tiếp trên Board (tạo, đổi tên, xóa)
- CRUD Card trực tiếp trên Board (tạo, xem chi tiết, cập nhật, xóa)
- Card Detail Modal
- Drag and Drop: List và Card với optimistic update
- Kiểm thử TC01–TC08

**Kết quả:** Board Kanban hoạt động đầy đủ, drag-and-drop cơ bản hoạt động

---

### Tuần 8 — Hoàn thiện (18/05 – 24/05/2026)
**Giai đoạn:** Frontend – Card Detail, System Testing

**Đã thực hiện:**
- Board Member Management UI: xem/thêm member vào Board
- Card Detail – Due Date: Date Picker, hiển thị trạng thái overdue/upcoming
- Card Detail – Checklist: CRUD đầy đủ, Progress Bar
- Card Detail – Card Member: gán/hủy thành viên, avatar trên card
- Kiểm thử toàn hệ thống, ghi nhận lỗi

**Kết quả:** Hoàn thiện tính năng nghiệp vụ cốt lõi; nhận diện 4 vấn đề cần giải quyết tiếp

---

## Phân bổ thời gian theo giai đoạn

```
Tuần 1-2  [██░░░░░░░░░░░░░░]  Setup & Design       (2/8 tuần = 25%)
Tuần 3-5  [░░███████░░░░░░░]  Backend Development  (3/8 tuần = 37.5%)
Tuần 6-8  [░░░░░░░░████████]  Frontend Development (3/8 tuần = 37.5%)
```

---

## Số lượng test cases theo tuần

| Tuần | Test Cases | Module kiểm thử |
|---|---|---|
| W3 | TC01–TC14 (14 cases) | Authentication |
| W4 | TC01–TC16 (16 cases) | Workspace, Board, List |
| W5 | TC17–TC25 (9 cases) | Card, Checklist |
| W6 | TC01–TC09 (9 cases) | Auth UI + Dashboard UI |
| W7 | TC01–TC08 (8 cases) | Workspace Detail + Board Kanban |
| W8 | System testing tổng thể | Toàn hệ thống |

# Tuần 6 — 04/05/2026 – 10/05/2026

## Mục tiêu tuần 6
- Phát triển Frontend (NextJS) — bắt đầu giai đoạn UI
- Authentication UI: Sign Up, Confirm Sign Up, Sign In
- Dashboard UI: Workspace và Board list
- Tích hợp hoàn chỉnh với các API Authentication, Workspace, Board

## Công việc đã thực hiện

### 1. Authentication UI

**Màn hình Sign Up (`/sign-up`):**
- Form nhập thông tin: `fullName`, `email`, `password`, `confirmPassword`
- Validation phía client trước khi gửi request
- Tích hợp API `POST /api/v1/auth/sign-up`
- Điều hướng sang Confirm Sign Up sau khi đăng ký thành công

**Màn hình Confirm Sign Up (`/confirm-sign-up`):**
- Form nhập mã OTP xác nhận qua email
- Tích hợp API `POST /api/v1/auth/confirm-sign-up`
- Tích hợp API `POST /api/v1/auth/resend-code` (nút "Let's re-send code")
- Điều hướng sang Sign In sau khi xác nhận thành công

**Màn hình Sign In (`/sign-in`):**
- Form login: `email`, `password`
- Tích hợp API `POST /api/v1/auth/sign-in`
- Lưu token bằng HTTP-only cookie
- Điều hướng sang Dashboard sau khi đăng nhập thành công

**Giao diện:** Dark theme, tên app "Nello" với logo

### 2. Dashboard (Workspace + Board list)

**Màn hình Dashboard:**
- Hiển thị danh sách Workspace theo user
- Hiển thị Board theo từng Workspace
- Xử lý empty state khi chưa có dữ liệu
- Tích hợp API `GET /api/v1/workspaces`

**Xử lý session và authentication flow:**
- Gọi API `GET /api/v1/users/me` khi vào Dashboard (kiểm tra session)
- Tự động refresh token qua `POST /api/v1/auth/refresh-token`
- Redirect về Sign In nếu session không hợp lệ

### 3. Workspace & Board Management

**Tạo Workspace mới:**
- Form: workspace name, workspace type (category), workspace description
- Tích hợp API `POST /api/v1/workspaces`
- Lấy danh sách category từ `GET /api/v1/workspace-categories`
- Cập nhật UI sau khi tạo thành công

**Tạo Board mới:**
- Form: chọn background (8 ảnh từ `GET /api/v1/images`), board title, chọn workspace
- Tích hợp API `POST /api/v1/boards`
- Cập nhật danh sách board realtime

### 4. Kiểm thử tích hợp Frontend

| TC | Kịch bản | Expected |
|---|---|---|
| TC01 | Đăng ký tài khoản thành công | HTTP 200, user tạo, gửi OTP email |
| TC02 | Xác nhận đăng ký thành công | HTTP 200, chuyển trạng thái verified |
| TC03 | Đăng nhập thành công | HTTP 200, set cookie token, redirect Dashboard |
| TC04 | Đăng ký user đã tồn tại | HTTP 409 "User already exists" |
| TC05 | Đăng nhập không thành công | HTTP 401 Unauthorized |
| TC06 | Xác nhận OTP sai | HTTP 400 "Invalid verification code" |
| TC07 | Load Dashboard thành công | HTTP 200, danh sách workspace hiển thị |
| TC08 | Tạo Workspace thành công | Workspace tạo và hiển thị trên UI |
| TC09 | Tạo Board thành công | Board hiển thị đúng trong workspace |

## Kết quả
- Hoàn thiện 3 màn hình xác thực (Sign Up, Confirm Sign Up, Sign In)
- Tích hợp đầy đủ luồng Authentication với API (TC01–TC03)
- Dashboard và các chức năng tạo Workspace, Board xây dựng và tích hợp thành công
- Hệ thống hoàn thiện luồng người dùng từ đăng ký → đăng nhập → Dashboard
- Cơ chế xác thực phiên làm việc và xử lý trạng thái lỗi hoàn chỉnh

## Kế hoạch tuần 7
- Workspace Detail UI (thông tin workspace, board list, member list)
- Board UI theo mô hình Kanban (List columns + Card)
- Drag-and-drop cho List và Card
- Quản lý Member trong Workspace

# Tuần 3 — 13/04/2026 – 19/04/2026

## Mục tiêu tuần 3
- Tích hợp xác thực người dùng với AWS Cognito
- Xây dựng các API xác thực cho backend
- Kiểm thử và hoàn thiện luồng đăng nhập/đăng ký

## Công việc đã thực hiện

### 1. Tích hợp AWS Cognito

**Cấu hình Cognito:**
- Tạo User Pool trên AWS Cognito để quản lý người dùng
- Cấu hình App Client cho backend (không sử dụng Hosted UI)
- Password policy: tối thiểu 8 ký tự, ít nhất 1 chữ hoa, 1 ký tự đặc biệt
- Cấu hình xác thực email

**Tích hợp AWS SDK vào Spring Boot:**
- Tạo `CognitoConfig.java`: Bean `CognitoIdentityProviderClient` với region `AP_SOUTHEAST_1`
- Tạo `CognitoService.java` (implements `ICognitoService`):
  - `signIn()`: Gọi `InitiateAuthRequest` với `USER_PASSWORD_AUTH`
  - `refreshToken()`: Gọi `InitiateAuthRequest` với `REFRESH_TOKEN_AUTH`
  - `resendCode()`: Gọi `ResendConfirmationCode`
- Xử lý exception từ Cognito: `UserNotConfirmedException`, `NotAuthorizedException`

**Chức năng đăng ký:**
- `signUp()` trong `CognitoService`: tạo user trên Cognito
- `confirmSignUp()`: xác nhận tài khoản bằng OTP

**Chức năng đăng nhập:**
- `signIn()` trong `AuthService`: gọi Cognito → nhận JWT → lưu vào HttpOnly Cookie
- Access token: cookie `ACCESS_TOKEN_FIELD_NAME`, HttpOnly, maxAge = token expiry
- Refresh token: cookie `REFRESH_TOKEN_FIELD_NAME`, HttpOnly

### 2. Xây dựng API xác thực (`/api/v1/auth`)

| API | Method | Mô tả |
|---|---|---|
| /sign-up | POST | Input: fullName, email, password. Tạo user trên Cognito |
| /confirm-sign-up | POST | Input: username, confirmationCode. Xác thực tài khoản |
| /sign-in | POST | Input: email, password. Trả về token qua HttpOnly Cookie |
| /refresh-token | POST | Lấy refresh token từ cookie, cấp mới access token |
| /resend-code | POST | Input: username. Gửi lại mã xác nhận |

**Thiết kế controller (`/api/v1/auth`):**
- Áp dụng DTO + Validation cho dữ liệu đầu vào
- Tích hợp guard/middleware kiểm tra token trong request
- Chuẩn hóa response API theo format chung

### 3. Kiểm thử luồng đăng nhập/đăng ký (14 test cases)

| TC | API | Kịch bản | Kết quả mong đợi |
|---|---|---|---|
| 1 | POST /sign-up | Dữ liệu hợp lệ | Tạo user trên Cognito thành công |
| 2 | POST /sign-up | Email đã tồn tại | HTTP 409, "User already exists" |
| 3 | POST /sign-up | Thiếu field/sai format | HTTP 400, validation |
| 4 | POST /confirm-sign-up | Mã hợp lệ | HTTP 200, xác thực thành công |
| 5 | POST /confirm-sign-up | Mã sai/hết hạn | HTTP 400, lỗi |
| 6 | POST /sign-in | Thông tin hợp lệ | HTTP 200, access + refresh token |
| 7 | POST /sign-in | Sai username/password | HTTP 401 |
| 8 | POST /sign-in | User chưa confirm | HTTP 401, "User not confirmed" |
| 9 | POST /sign-in | Kiểm tra token HttpOnly Cookie | Cookie được set đúng |
| 10 | POST /refresh-token | Token hợp lệ | HTTP 200, cấp mới access token |
| 11 | POST /refresh-token | Thiếu refresh token | HTTP 401 |
| 12 | POST /refresh-token | Token không hợp lệ/hết hạn | HTTP 400 |
| 13 | POST /resend-code | User hợp lệ | Gửi mã thành công |
| 14 | POST /resend-code | User không tồn tại | Vẫn thành công (Cognito spec) |

## Kết quả
- Backend tích hợp thành công với AWS Cognito
- Đầy đủ API xác thực người dùng
- Luồng đăng ký và đăng nhập hoạt động ổn định

## Kế hoạch tuần 4
- Phát triển chức năng quản lý Workspace và Board
- Xây dựng API cho List (tạo, cập nhật, xóa)
- Kiểm thử các chức năng backend đã hoàn thành

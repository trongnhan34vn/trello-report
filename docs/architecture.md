# Kiến trúc hệ thống

## Mô hình tổng thể

Hệ thống áp dụng kiến trúc **Client-Server phân tách hoàn toàn** (decoupled Frontend/Backend):

```
┌─────────────────────┐         REST API          ┌──────────────────────┐
│   Client (NextJS)   │ ─────────────────────────▶ │  Backend (Spring Boot)│
│   "Nello" App       │ ◀─────────────────────────  │  Port 8080           │
└─────────────────────┘    JSON / HttpOnly Cookie  └──────────┬───────────┘
                                                              │
                                          ┌───────────────────┼───────────────────┐
                                          ▼                   ▼                   ▼
                                 ┌─────────────┐    ┌──────────────┐    ┌─────────────┐
                                 │ PostgreSQL  │    │ AWS Cognito  │    │   Flyway    │
                                 │ (AWS RDS)   │    │ (Auth/JWT)   │    │ (Migration) │
                                 └─────────────┘    └──────────────┘    └─────────────┘
```

---

## Các thành phần

### Frontend — NextJS (Nello)
- **Vai trò:** Giao diện người dùng; gọi API backend qua HTTP
- **Giao tiếp:** REST API (JSON), token lưu trong HttpOnly Cookie
- **Quản lý state:** Tự quản lý session (gọi `/api/v1/users/me` khi load)
- **Routing:** Server-side routing với NextJS (App Router)

### Backend — Spring Boot
- **Vai trò:** Xử lý nghiệp vụ; cung cấp RESTful API
- **Base path:** `/api/v1`
- **Xác thực:** Kiểm tra JWT từ HttpOnly Cookie trong mỗi request
- **Tầng:**
  - Controller → Service → Repository (JPA)
  - DTO + Validation cho dữ liệu đầu vào
  - Exception handler chuẩn hóa response

### Database — PostgreSQL (AWS RDS)
- **Vai trò:** Lưu trữ toàn bộ dữ liệu nghiệp vụ
- **Migration:** Flyway quản lý phiên bản schema (`V1__init.sql`)
- **Quan hệ:** Cascade delete theo cây Workspace → Board → List → Card

### Authentication — AWS Cognito
- **Vai trò:** Quản lý người dùng, xác thực và cấp JWT
- **User Pool:** Lưu thông tin đăng nhập, chính sách mật khẩu
- **App Client:** Backend giao tiếp qua AWS SDK (không dùng Hosted UI)
- **JWT:** Access token + Refresh token trả về cho backend, lưu vào HttpOnly Cookie

---

## Luồng xác thực

```
User                  Frontend               Backend               AWS Cognito
 │                       │                      │                       │
 │──── Nhập email/pw ───▶│                      │                       │
 │                       │── POST /auth/sign-in ▶│                       │
 │                       │                      │── InitiateAuth ───────▶│
 │                       │                      │◀── JWT (access/refresh)│
 │                       │◀── Set HttpOnly Cookie│                       │
 │◀── Redirect Dashboard ─│                      │                       │
 │                       │                      │                       │
 │──── Gọi API bất kỳ ──▶│                      │                       │
 │                       │── Request + Cookie ──▶│                       │
 │                       │                      │── Verify JWT ─────────▶│
 │                       │                      │◀── Valid               │
 │                       │◀── Response           │                       │
```

---

## Luồng refresh token

```
Frontend                              Backend                    AWS Cognito
    │                                     │                           │
    │── POST /auth/refresh-token ─────────▶│                           │
    │   (refresh_token từ cookie)          │── REFRESH_TOKEN_AUTH ────▶│
    │                                     │◀── New access token        │
    │◀── Set new access_token cookie ──────│                           │
```

---

## Luồng tạo Card (ví dụ nghiệp vụ)

```
Frontend                         Backend                      Database
    │                                │                             │
    │── POST /api/v1/cards ──────────▶│                             │
    │   { listId, title, position }   │── Verify JWT               │
    │                                │── Kiểm tra List tồn tại ───▶│
    │                                │◀── List found               │
    │                                │── Kiểm tra quyền user       │
    │                                │── INSERT card ─────────────▶│
    │                                │◀── card record              │
    │◀── HTTP 201 { cardId, ... } ───│                             │
```

---

## Cấu trúc thư mục dự án báo cáo

```
reports/
├── CLAUDE.md              # Hướng dẫn sinh báo cáo
├── AGENTS.md              # Cấu hình agent
├── V1__init.sql           # Database migration script
├── openapi.json           # OpenAPI 3.1 specification
├── src/
│   ├── main.tex           # File LaTeX chính
│   ├── content.tex        # Nội dung báo cáo (output)
│   ├── cover.tex          # Trang bìa
│   ├── thanks.tex         # Lời cảm ơn
│   └── images/            # Hình ảnh minh họa
├── weekly-reports/        # 8 file PDF báo cáo tuần
└── docs/                  # Tài liệu phân tích tổng hợp
```

---

## Quyết định kiến trúc chính

| Quyết định | Lựa chọn | Lý do |
|---|---|---|
| API style | RESTful | Dễ mở rộng, không phụ thuộc ngôn ngữ, phù hợp phân tán |
| Auth provider | AWS Cognito | Quản lý user chuyên dụng, hỗ trợ JWT, MFA, OAuth2 |
| Token storage | HttpOnly Cookie | Bảo mật hơn localStorage (không bị XSS đọc) |
| Database | PostgreSQL | RDBMS mạnh, hỗ trợ UUID, enum, cascade delete |
| DB migration | Flyway | Quản lý phiên bản schema, tích hợp tốt với Spring Boot |
| Position type | TEXT (string) | Linh hoạt cho drag-and-drop, không cần reindex số nguyên |
| Frontend | NextJS | SSR/CSR linh hoạt, routing tốt, React ecosystem |

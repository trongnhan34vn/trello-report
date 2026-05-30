# Công nghệ sử dụng

## Tổng hợp stack

| Tầng | Công nghệ | Phiên bản/Ghi chú |
|---|---|---|
| Backend framework | Spring Boot (Java) | REST API, JPA/Hibernate |
| Frontend framework | NextJS (React) | App Router |
| Database | PostgreSQL | Trên AWS RDS |
| Cloud DB | AWS RDS | Managed PostgreSQL |
| Authentication | AWS Cognito | User Pool + JWT |
| Auth SDK | AWS SDK for Java | CognitoIdentityProviderClient |
| DB Migration | Flyway | V1__init.sql |
| API Documentation | Swagger / OpenAPI 3.1 | springdoc-openapi |
| Token storage | HttpOnly Cookie | Access + Refresh token |
| Position tracking | String-based | Cho drag-and-drop |

---

## Chi tiết từng công nghệ

### 1. Spring Boot

**Giới thiệu:**  
Spring Boot là framework phát triển ứng dụng Java, xây dựng trên nền tảng Spring Framework. Spring Boot giảm thiểu cấu hình thủ công bằng cơ chế auto-configuration.

**Chức năng trong hệ thống:**
- Xây dựng RESTful API với `@RestController`, `@RequestMapping`
- Quản lý dependency injection qua Spring IoC Container
- Xử lý ORM với Spring Data JPA + Hibernate
- Validation dữ liệu đầu vào với Bean Validation (`@Valid`, `@NotNull`)
- Xử lý exception tập trung với `@ControllerAdvice`
- Tích hợp AWS SDK để giao tiếp với Cognito

**Vai trò trong hệ thống:**  
Backend chính, xử lý toàn bộ nghiệp vụ và cung cấp API cho Frontend.

---

### 2. NextJS

**Giới thiệu:**  
NextJS là framework React hỗ trợ Server-Side Rendering (SSR), Static Site Generation (SSG) và Client-Side Rendering (CSR). NextJS cung cấp hệ thống routing dựa trên cấu trúc thư mục.

**Chức năng trong hệ thống:**
- Xây dựng giao diện người dùng (dark theme, tên ứng dụng "Nello")
- Routing: `/sign-up`, `/sign-in`, `/confirm-sign-up`, `/workspaces/:id`, `/boards/:id`
- Gọi REST API backend và xử lý response
- Quản lý trạng thái loading, error, empty state
- Xử lý HttpOnly Cookie cho session management

**Vai trò trong hệ thống:**  
Frontend của ứng dụng, cung cấp giao diện cho người dùng cuối.

---

### 3. PostgreSQL

**Giới thiệu:**  
PostgreSQL là hệ quản trị cơ sở dữ liệu quan hệ mã nguồn mở, mạnh về tính toàn vẹn dữ liệu, hỗ trợ kiểu dữ liệu phong phú (UUID, ENUM, TEXT, JSONB).

**Chức năng trong hệ thống:**
- Lưu trữ toàn bộ dữ liệu nghiệp vụ: users, workspaces, boards, lists, cards, checklists
- Sử dụng UUID làm khóa chính cho các bảng chính
- Custom ENUM type: `role_name_enum` (ADMIN, MEMBER, VIEWER)
- Cascade delete: xóa workspace tự động xóa boards, lists, cards liên quan
- Unique constraint: đảm bảo mỗi user chỉ có một vai trò trong mỗi workspace/board/card

**Vai trò trong hệ thống:**  
Lưu trữ dữ liệu chính của hệ thống.

---

### 4. AWS RDS (Relational Database Service)

**Giới thiệu:**  
AWS RDS là dịch vụ cơ sở dữ liệu quan hệ được quản lý hoàn toàn bởi Amazon Web Services. RDS loại bỏ gánh nặng vận hành database (cài đặt, backup, patching).

**Chức năng trong hệ thống:**
- Host PostgreSQL instance cho backend Spring Boot
- Tự động backup và restore
- Bảo mật thông qua VPC và IAM
- Hỗ trợ mở rộng tài nguyên (scaling) khi cần

**Vai trò trong hệ thống:**  
Hạ tầng cloud cho PostgreSQL, đảm bảo tính sẵn sàng và bảo mật của database.

---

### 5. AWS Cognito

**Giới thiệu:**  
AWS Cognito là dịch vụ quản lý xác thực người dùng (authentication) và phân quyền (authorization) của Amazon Web Services.

**Thành phần chính:**
- **User Pool:** Quản lý danh sách người dùng, thông tin đăng nhập, chính sách mật khẩu
- **App Client:** Điểm tích hợp cho backend (không dùng Hosted UI)
- **JWT Token:** Cognito cấp 3 loại token: access token, id token, refresh token

**Chức năng trong hệ thống:**
- Xử lý đăng ký tài khoản mới (tạo user trong User Pool)
- Gửi mã OTP xác nhận qua email
- Xác thực đăng nhập (email + password) và cấp JWT
- Làm mới access token từ refresh token
- Gửi lại mã xác nhận khi cần

**Cấu hình:**
- Password policy: tối thiểu 8 ký tự, 1 chữ hoa, 1 ký tự đặc biệt
- Xác thực email bắt buộc
- Không sử dụng Hosted UI (backend tự gọi Cognito SDK)

**Luồng xác thực:**  
Người dùng → Cognito → JWT → Client gọi API kèm token

**Vai trò trong hệ thống:**  
Quản lý toàn bộ vòng đời xác thực người dùng, tách biệt khỏi logic nghiệp vụ backend.

---

### 6. JWT (JSON Web Token)

**Giới thiệu:**  
JWT là chuẩn mở (RFC 7519) để truyền tải thông tin dưới dạng JSON object được ký số. JWT được AWS Cognito cấp sau khi xác thực thành công.

**Chức năng trong hệ thống:**
- **Access token:** Dùng để xác thực mỗi API request (ngắn hạn)
- **Refresh token:** Dùng để làm mới access token (dài hạn)
- Backend kiểm tra JWT hợp lệ trong HttpOnly Cookie trước khi xử lý request
- JWT chứa thông tin user (sub = cognito_id)

**Vai trò trong hệ thống:**  
Cơ chế xác thực stateless giữa Frontend và Backend.

---

### 7. Flyway

**Giới thiệu:**  
Flyway là công cụ quản lý phiên bản schema cơ sở dữ liệu (database migration). Flyway theo dõi lịch sử thay đổi schema qua các file migration có đánh số phiên bản.

**Chức năng trong hệ thống:**
- Tự động thực thi script SQL khi ứng dụng khởi động
- File migration: `V1__init.sql` — tạo toàn bộ schema ban đầu
- Đảm bảo schema nhất quán giữa các môi trường (dev, staging, prod)

**Vai trò trong hệ thống:**  
Quản lý và kiểm soát phiên bản database schema.

---

### 8. RESTful API / OpenAPI 3.1

**Giới thiệu:**  
REST (Representational State Transfer) là kiểu kiến trúc API sử dụng giao thức HTTP. OpenAPI (Swagger) là chuẩn mô tả REST API.

**Lý do chọn RESTful API:**
- Dễ mở rộng và tích hợp
- Không phụ thuộc ngôn ngữ lập trình
- Phù hợp hệ thống phân tán
- Hỗ trợ đầy đủ HTTP methods: GET, POST, PUT, PATCH, DELETE

**Chức năng trong hệ thống:**
- API Documentation tự động từ Swagger (springdoc-openapi)
- OpenAPI 3.1 specification: `openapi.json`
- 14 nhóm endpoint, base path `/api/v1`
- Chuẩn hóa response format cho toàn bộ API

**Vai trò trong hệ thống:**  
Giao thức giao tiếp chuẩn giữa Frontend và Backend; tài liệu API cho phát triển và kiểm thử.

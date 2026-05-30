# Tuần 1 — 30/03/2026 – 05/04/2026

## Mục tiêu tuần 1
- Khởi tạo dự án và thiết lập môi trường phát triển
- Nghiên cứu và lựa chọn công nghệ

## Công việc đã thực hiện

### 1. Khởi tạo dự án Spring Boot
- Tạo project Spring Boot với các dependency cần thiết
- Cấu hình kết nối PostgreSQL

### 2. Thiết lập PostgreSQL trên AWS RDS
- Tạo instance RDS PostgreSQL
- Cấu hình VPC, security group
- Kết nối backend với RDS

### 3. Cấu hình Flyway Migration
- Tích hợp Flyway vào Spring Boot
- Chuẩn bị cấu trúc thư mục migration

### 4. Thiết lập Swagger / OpenAPI
- Tích hợp Swagger UI để document API
- Cấu hình base path `/api/v1`

### 5. Nghiên cứu công nghệ
- **Spring Boot:** Framework backend chính
- **PostgreSQL / AWS RDS:** Cơ sở dữ liệu quan hệ được quản lý bởi AWS
- **AWS Cognito:** Dịch vụ xác thực người dùng (User Pool, JWT)
- **RESTful API:** Lựa chọn thiết kế API (dễ mở rộng, không phụ thuộc ngôn ngữ, phù hợp hệ thống phân tán)

## Kết quả
- Dự án Spring Boot khởi động thành công
- Kết nối RDS hoạt động
- Swagger UI hiển thị được tại localhost:8080

## Kế hoạch tuần 2
- Thiết kế API documentation đầy đủ
- Thiết kế database schema

# Tuần 2 — 06/04/2026 – 12/04/2026

## Mục tiêu tuần 2
- Thiết kế toàn bộ API documentation
- Thiết kế database schema

## Công việc đã thực hiện

### 1. Thiết kế API với Swagger
Thiết kế và document đầy đủ các nhóm API:
- **User:** endpoints quản lý người dùng
- **Authentication:** endpoints xác thực (sign-up, sign-in, refresh-token, ...)
- **Workspace:** endpoints quản lý workspace
- **Board:** endpoints quản lý board
- **List:** endpoints quản lý list
- **Card:** endpoints quản lý card
- **WorkspaceCategory:** endpoints lấy danh mục workspace

Swagger UI hiển thị đầy đủ 2 nhóm ảnh chụp màn hình (Swagger 1, Swagger 2)

**Lý do chọn RESTful API:**
- Dễ mở rộng và tích hợp
- Không phụ thuộc ngôn ngữ lập trình
- Phù hợp hệ thống phân tán

### 2. Thiết kế Database Schema
Thiết kế schema PostgreSQL hoàn chỉnh (trở thành file V1__init.sql):

**Các bảng:**
- `users` (UUID, cognito_id, full_name, email, avatar_url, bio, phone, address)
- `workspace_categories` (8 loại: Sales CRM, Education, HR, Operation, Marketing, IT, Small Business, Other)
- `workspaces` (UUID, name, description, category_id)
- `boards` (UUID, name, background_url, workspace_id)
- `lists` (UUID, name, board_id, position)
- `cards` (UUID, title, description, start_date, due_date, is_completed, position, list_id)
- `checklists` (UUID, name, card_id, position)
- `checklist_items` (UUID, name, checklist_id, position, is_completed, due_date)
- `workspace_members` (user_id, workspace_id, role_id)
- `board_members` (user_id, board_id, role_id)
- `card_members` (user_id, card_id)
- `roles` (ADMIN=1, MEMBER=2, VIEWER=3)
- `images` (8 ảnh nền board)

**Ràng buộc:**
- Unique: `(user_id, workspace_id)`, `(user_id, board_id)`, `(user_id, card_id)`
- Cascade delete theo cây: workspace → board → list → card → checklist → checklist_item

### 3. Nghiên cứu AWS Cognito
- **User Pool:** Quản lý người dùng và đăng nhập
- **Identity Pool:** Cấp quyền truy cập AWS resources
- **JWT Token:** access token, id token, refresh token
- **Luồng xác thực:** Người dùng → Cognito → JWT → Client gọi API kèm token
- **Tính năng:** Đăng nhập OAuth2/OpenID Connect, MFA, đăng nhập social

### 4. Nghiên cứu AWS RDS
- Dịch vụ cơ sở dữ liệu quan hệ managed bởi AWS
- Hỗ trợ PostgreSQL, MySQL, MariaDB, Oracle, SQL Server
- Tính năng: auto backup, scaling, High Availability (Multi-AZ), bảo mật VPC + IAM

## Kết quả
- API documentation hoàn chỉnh trên Swagger
- Database schema thiết kế xong (sẵn sàng migrate)

## Kế hoạch tuần 3
- Tích hợp xác thực người dùng với AWS Cognito
- Xây dựng các API xác thực cho backend
- Kiểm thử và hoàn thiện luồng đăng nhập/đăng ký

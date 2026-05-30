# Project Knowledge — Hệ thống Quản lý Công việc (Nello)

> Tổng hợp từ 8 báo cáo tuần (W1–W8), V1__init.sql và openapi.json.
> Dùng làm ngữ cảnh nền để viết báo cáo, không cần đọc lại PDF.

---

## 1. Tổng quan hệ thống

**Tên đề tài:** Hệ thống Quản lý Công việc  
**Tên ứng dụng frontend:** Nello  
**Sinh viên:** Nguyễn Trọng Nhân – 202490073 – CNTT 1.1 – K69  
**GVHD:** Th.S Phạm Quang Hiếu  
**Trường:** Đại học Bách Khoa Hà Nội – SOICT  
**Thời gian thực hiện:** 30/03/2026 – 24/05/2026 (8 tuần)

**Mô tả:**  
Hệ thống quản lý công việc theo mô hình Kanban, lấy cảm hứng từ Trello. Người dùng có thể tổ chức công việc theo Workspace → Board → List → Card, với các tính năng như checklist, due date, phân quyền thành viên và kéo thả.

---

## 2. Kiến trúc hệ thống

### Mô hình tổng thể

```
Client (NextJS)  →  Backend API (Spring Boot)  →  PostgreSQL (AWS RDS)
                                ↕
                        AWS Cognito (Auth)
```

- **Frontend:** NextJS (React) — tên ứng dụng: Nello, giao diện dark theme
- **Backend:** Spring Boot (Java), RESTful API, port 8080
- **Database:** PostgreSQL trên AWS RDS
- **Authentication:** AWS Cognito (User Pool + JWT)
- **API Documentation:** Swagger / OpenAPI 3.1.0
- **DB Migration:** Flyway (file V1__init.sql)
- **Token storage:** HttpOnly Cookie (access token + refresh token)

### Luồng xác thực

```
User → Frontend → POST /api/v1/auth/sign-in → Backend → AWS Cognito
                                                       ↓
                                            JWT (access + refresh token)
                                                       ↓
                                          Lưu vào HttpOnly Cookie
```

---

## 3. Công nghệ sử dụng

| Thành phần | Công nghệ |
|---|---|
| Backend | Spring Boot (Java) |
| Frontend | NextJS (React) |
| Database | PostgreSQL |
| Cloud DB | AWS RDS |
| Authentication | AWS Cognito |
| Auth protocol | JWT (OAuth2 / OpenID Connect) |
| API Doc | Swagger / OpenAPI 3.1 |
| DB Migration | Flyway |
| Token storage | HttpOnly Cookie |
| Position tracking | String-based position (drag-and-drop) |

### AWS Cognito
- **User Pool:** Quản lý người dùng và đăng nhập
- **App Client:** Cấu hình cho backend (không dùng Hosted UI)
- **Password policy:** Tối thiểu 8 ký tự, 1 hoa, 1 ký tự đặc biệt
- **JWT Token:** access token, id token, refresh token
- **Luồng:** Người dùng → Cognito → JWT → Client gọi API kèm token
- **SDK:** AWS SDK tích hợp vào Spring Boot (`CognitoIdentityProviderClient`)

### AWS RDS
- Dịch vụ cơ sở dữ liệu quan hệ được quản lý bởi AWS
- Hệ quản trị: PostgreSQL
- Tính năng: Auto backup, scaling, High Availability (Multi-AZ), bảo mật VPC + IAM

---

## 4. Các module chức năng

### 4.1 Authentication Module
**Endpoint base:** `/api/v1/auth`

| API | Method | Mô tả |
|---|---|---|
| /sign-up | POST | Đăng ký tài khoản mới qua Cognito |
| /confirm-sign-up | POST | Xác nhận tài khoản bằng mã OTP email |
| /sign-in | POST | Đăng nhập, nhận JWT, lưu vào HttpOnly Cookie |
| /refresh-token | POST | Làm mới access token từ refresh token trong cookie |
| /resend-code | POST | Gửi lại mã xác nhận |

**Input sign-up:** `fullName`, `email`, `password`  
**Input sign-in:** `email`, `password`  
**Response sign-in:** Set HttpOnly Cookie chứa `access_token` + `refresh_token`

### 4.2 User Module
**Endpoint base:** `/api/v1/users`

| API | Method | Mô tả |
|---|---|---|
| /users | GET | Tìm kiếm user theo email hoặc tên (loại trừ current user) |
| /users/me | GET | Lấy thông tin user hiện tại |

### 4.3 Workspace Module
**Endpoint base:** `/api/v1/workspaces`

| API | Method | Mô tả |
|---|---|---|
| / | POST | Tạo workspace mới (owner = current user) |
| / | GET | Lấy danh sách workspace thuộc user hiện tại |
| /:id | GET | Lấy chi tiết workspace |
| /:id/boards | GET | Lấy danh sách board thuộc workspace |
| /:id/members | GET | Lấy danh sách member trong workspace |

**WorkspaceCategory** (8 loại): Sales CRM, Education, Human Resources, Operation, Marketing, IT, Small Business, Other

### 4.4 Workspace Member Module
**Endpoint base:** `/api/v1/workspaces-members`

| API | Method | Mô tả |
|---|---|---|
| /:id | POST | Thêm member vào workspace (bằng email) |

### 4.5 Board Module
**Endpoint base:** `/api/v1/boards`

| API | Method | Mô tả |
|---|---|---|
| / | POST | Tạo board mới trong workspace |
| /:id | GET | Lấy chi tiết board (kèm list, card) |
| /:id | PUT | Cập nhật board |
| /:id | DELETE | Xóa board |

**Board background:** Chọn từ 8 ảnh nền (`/api/v1/images`)

### 4.6 Board Member Module
**Endpoint base:** `/api/v1/boards-members` / `/api/v1/board-members`

| API | Method | Mô tả |
|---|---|---|
| / | POST | Thêm member vào board (theo userIds + roleId) |
| ?boardId=&search= | GET | Tìm member trong board |

### 4.7 List Module
**Endpoint base:** `/api/v1/lists`

| API | Method | Mô tả |
|---|---|---|
| / | POST | Tạo list trong board |
| /:id | PATCH | Cập nhật tên và vị trí list |
| /:id | DELETE | Xóa list |
| ?boardId= | GET | Lấy danh sách list theo board (kèm thứ tự) |

### 4.8 Card Module
**Endpoint base:** `/api/v1/cards`

| API | Method | Mô tả |
|---|---|---|
| / | POST | Tạo card trong list |
| /:id | GET | Lấy chi tiết card |
| /:id | PATCH | Cập nhật card (title, description, startDate, dueDate, position, listId, isCompleted) |
| /:id | DELETE | Xóa card |
| ?boardId= | GET | Lấy danh sách card theo board, sắp xếp theo position |

**Di chuyển card (drag-and-drop):** PATCH /:id với `listId` (list đích) + `position` (vị trí mới)

### 4.9 Checklist Module
**Endpoint base:** `/api/v1/checklists`

| API | Method | Mô tả |
|---|---|---|
| / | POST | Tạo checklist trong card |
| /:id | PATCH | Cập nhật checklist (name, position) |
| /:id | DELETE | Xóa checklist |
| ?cardId= | GET | Lấy danh sách checklist thuộc card (kèm items) |

### 4.10 Checklist Item Module
**Endpoint base:** `/api/v1/checklist-items`

| API | Method | Mô tả |
|---|---|---|
| / | POST | Tạo item trong checklist |
| /:id | PATCH | Cập nhật item (name, position, dueDate, isCompleted) |
| /:id | DELETE | Xóa item |

### 4.11 Card Member Module
**Endpoint base:** `/api/v1/card-members`

| API | Method | Mô tả |
|---|---|---|
| / | POST | Gán thành viên vào card |
| /:id | DELETE | Hủy gán thành viên khỏi card |

### 4.12 Image Module
**Endpoint:** `GET /api/v1/images`  
Trả về danh sách 8 ảnh nền dùng cho board background.

### 4.13 WorkspaceCategory Module
**Endpoint:** `GET /api/v1/workspace-categories`  
Trả về danh sách 8 loại workspace.

### 4.14 Role Module
**Endpoint:** `GET /api/v1/roles`  
Trả về danh sách role: ADMIN, MEMBER, VIEWER.

---

## 5. Database Schema

### Bảng chính

| Bảng | Khóa chính | Mô tả |
|---|---|---|
| users | UUID | Thông tin người dùng |
| workspaces | UUID | Không gian làm việc |
| boards | UUID | Bảng công việc |
| lists | UUID | Danh sách trong board |
| cards | UUID | Thẻ công việc trong list |
| checklists | UUID | Danh sách kiểm tra trong card |
| checklist_items | UUID | Mục trong checklist |
| workspace_members | UUID | Thành viên workspace |
| board_members | UUID | Thành viên board |
| card_members | UUID | Thành viên được gán cho card |
| roles | INT | Vai trò: ADMIN(1), MEMBER(2), VIEWER(3) |
| workspace_categories | INT | Phân loại workspace (8 loại) |
| images | INT | Ảnh nền board (8 ảnh) |

### Chi tiết bảng users
```sql
users (
  id UUID PRIMARY KEY,
  cognito_id VARCHAR(255) UNIQUE NOT NULL,
  full_name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  avatar_url VARCHAR(255),
  bio TEXT,
  phone VARCHAR(255),
  address VARCHAR(255),
  deleted_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
)
```

### Chi tiết bảng cards
```sql
cards (
  id UUID PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  description TEXT,
  start_date TIMESTAMP,
  due_date TIMESTAMP,
  is_completed BOOLEAN DEFAULT FALSE,
  position TEXT NOT NULL,
  list_id UUID REFERENCES lists(id) ON DELETE CASCADE,
  created_by UUID REFERENCES users(id),
  updated_by UUID REFERENCES users(id),
  created_at TIMESTAMP,
  updated_at TIMESTAMP
)
```

### Quan hệ giữa các bảng

```
users ──< workspace_members >── workspaces ──< boards ──< lists ──< cards
                                                 │                    │
                                       board_members           checklist_items
                                                                     │
                                                                checklists
                                                                     │
                                                              card_members
```

- `workspaces` CASCADE DELETE → `boards` → `lists` → `cards`
- `cards` CASCADE DELETE → `checklists` → `checklist_items`
- Unique constraints: `(user_id, workspace_id)`, `(user_id, board_id)`, `(user_id, card_id)`
- Position: kiểu `TEXT` (string-based) để hỗ trợ drag-and-drop linh hoạt

### Enum & Dữ liệu mặc định

**Roles:**
- 1: ADMIN
- 2: MEMBER
- 3: VIEWER

**WorkspaceCategories:**
- Sales CRM, Education, Human Resources, Operation, Marketing, IT, Small Business, Other

**Images:** 8 ảnh nền board (`/images/board_background/background_1.jpg` đến `background_8.jpg`)

---

## 6. Frontend (NextJS — Nello)

### Các màn hình đã triển khai

| Route | Màn hình | Mô tả |
|---|---|---|
| /sign-up | Sign Up | Form đăng ký (fullName, email, password, confirmPassword) |
| /confirm-sign-up | Confirm Email | Nhập OTP xác nhận email |
| /sign-in | Sign In | Form đăng nhập (email, password) |
| / (Dashboard) | Dashboard | Danh sách Workspace + Board của user |
| /workspaces/:id | Workspace Detail | Thông tin workspace, board list, member list |
| /boards/:id | Board (Kanban) | Màn hình Kanban: List columns + Card |

### Tính năng UI đã hoàn thiện (tính đến W8)

- Authentication flow đầy đủ (sign-up → OTP → sign-in → dashboard)
- Token auto-refresh khi gọi Dashboard
- Tạo Workspace (chọn category, mô tả)
- Tạo Board (chọn background từ 8 ảnh)
- Workspace Detail: Board list + Member list
- Mời Member vào Workspace qua email
- Board Kanban: Tạo/xóa List, Tạo/cập nhật/xóa Card
- Card Detail Modal: Due Date, Checklist, Card Member
- Board Member Management: Thêm thành viên từ danh sách workspace
- Drag and Drop: List và Card (có optimistic update)
- Progress bar cho Checklist

---

## 7. Phân quyền

| Role | Quyền |
|---|---|
| ADMIN | Toàn quyền: tạo, sửa, xóa, quản lý member |
| MEMBER | Tạo, sửa Card, List trong Board |
| VIEWER | Chỉ xem (chưa hoàn thiện kiểm soát ở W8) |

---

## 8. Các chức năng đã hoàn thành (tính đến W8)

### Backend (hoàn chỉnh)
- [x] Authentication: sign-up, confirm, sign-in, refresh-token, resend-code
- [x] User: tìm kiếm user, lấy thông tin bản thân
- [x] Workspace CRUD + member management
- [x] Board CRUD + member management
- [x] List CRUD với position (drag-and-drop)
- [x] Card CRUD với position, due date, assignee
- [x] Checklist CRUD
- [x] ChecklistItem CRUD với is_completed
- [x] Card Member: gán/hủy thành viên
- [x] Images, Roles, WorkspaceCategories endpoints

### Frontend (hoàn chỉnh)
- [x] Authentication UI (Sign Up, Confirm, Sign In)
- [x] Dashboard (Workspace + Board list)
- [x] Workspace Detail (Board list + Member list)
- [x] Invite Member to Workspace
- [x] Board Kanban (List columns + Cards)
- [x] Card Detail Modal (Due Date, Checklist, Members)
- [x] Board Member Management
- [x] Drag and Drop (List + Card) — có bug vị trí đầu/cuối

---

## 9. Các vấn đề còn tồn tại (W8)

| Vấn đề | Mô tả |
|---|---|
| Drag-and-drop bug | Lỗi khi kéo vào đầu/cuối danh sách; sai lệch vị trí giữa Frontend và Backend |
| Realtime Websocket | Chưa triển khai; các thay đổi từ user khác không cập nhật real-time |
| VIEWER role | Chưa giới hạn đầy đủ thao tác chỉnh sửa ở cả Frontend và Backend |
| Staging deployment | Chưa hoàn tất; vấn đề đồng nhất domain Frontend/Backend cho HttpOnly Cookie |

---

## 10. Hướng phát triển tiếp theo

- Sửa bug Drag and Drop (beforeId/afterId algorithm)
- Triển khai Realtime Websocket (đồng bộ Card tạo/cập nhật, Checklist, Drag-and-drop)
- Hoàn thiện phân quyền VIEWER
- Quản lý nhãn dán (Tag): tạo, gắn, tìm kiếm theo Tag
- Hệ thống thông báo (Notification)
- Nhật ký hoạt động (Activity Log)
- Triển khai Staging (xử lý HttpOnly Cookie cross-domain)

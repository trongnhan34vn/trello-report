# Các module chức năng

## Tổng hợp module

| Module | Base URL | Tuần triển khai |
|---|---|---|
| Authentication | /api/v1/auth | W3 (backend), W6 (frontend) |
| User | /api/v1/users | W3 (backend), W6 (frontend) |
| Workspace | /api/v1/workspaces | W4 (backend), W6 (frontend) |
| Workspace Member | /api/v1/workspaces-members | W4 (backend), W7 (frontend) |
| WorkspaceCategory | /api/v1/workspace-categories | W4 (backend) |
| Board | /api/v1/boards | W4 (backend), W6 (frontend) |
| Board Member | /api/v1/boards-members | W4 (backend), W8 (frontend) |
| List | /api/v1/lists | W4 (backend), W7 (frontend) |
| Card | /api/v1/cards | W5 (backend), W7 (frontend) |
| Checklist | /api/v1/checklists | W5 (backend), W8 (frontend) |
| Checklist Item | /api/v1/checklist-items | W5 (backend), W8 (frontend) |
| Card Member | /api/v1/card-members | W5 (backend), W8 (frontend) |
| Image | /api/v1/images | W4 (backend) |
| Role | /api/v1/roles | W4 (backend) |

---

## 1. Module Authentication

**Mục đích:** Quản lý toàn bộ vòng đời xác thực người dùng thông qua AWS Cognito.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /auth/sign-up | POST | fullName, email, password | HTTP 200 | Tạo user trên Cognito, gửi OTP email |
| /auth/confirm-sign-up | POST | username, confirmationCode | HTTP 200 | Xác nhận tài khoản bằng OTP |
| /auth/sign-in | POST | email, password | HttpOnly Cookie (access+refresh) | Đăng nhập, nhận JWT |
| /auth/refresh-token | POST | refresh_token (cookie) | HttpOnly Cookie (new access token) | Làm mới access token |
| /auth/resend-code | POST | username | HTTP 200 | Gửi lại mã OTP |

**Luồng sign-up:**
1. Client gửi fullName, email, password
2. Backend gọi Cognito `signUp()`
3. Cognito tạo user, gửi OTP qua email
4. Client gửi OTP → Backend gọi Cognito `confirmSignUp()`
5. User được kích hoạt, có thể đăng nhập

**Luồng sign-in:**
1. Client gửi email, password
2. Backend gọi Cognito `initiateAuth()` với `USER_PASSWORD_AUTH`
3. Cognito trả về JWT (access token + refresh token)
4. Backend set 2 HttpOnly Cookie: `ACCESS_TOKEN_FIELD_NAME`, `REFRESH_TOKEN_FIELD_NAME`
5. Client redirect về Dashboard

**Bảo mật:**
- Token lưu trong HttpOnly Cookie (không thể đọc bằng JavaScript → bảo vệ khỏi XSS)
- Access token: thời hạn ngắn
- Refresh token: thời hạn dài, dùng để gia hạn session

---

## 2. Module User

**Mục đích:** Quản lý thông tin người dùng trong hệ thống.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /users | GET | search (query param: email hoặc name) | Danh sách user | Tìm kiếm user, loại trừ current user |
| /users/me | GET | — | Thông tin user hiện tại | Lấy profile từ token |

**Mục đích /users (search):** Được gọi khi mời member vào Workspace/Board, cho phép tìm kiếm user để thêm vào.

---

## 3. Module Workspace

**Mục đích:** Quản lý không gian làm việc — đơn vị tổ chức cấp cao nhất.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /workspaces | POST | name, categoryId, description | Workspace mới | Tạo workspace, owner = user hiện tại |
| /workspaces | GET | — | Danh sách workspace | Lấy workspace thuộc user hiện tại |
| /workspaces/:id | GET | workspaceId | Chi tiết workspace | Tên, mô tả, category |
| /workspaces/:id/boards | GET | workspaceId | Danh sách board | Boards thuộc workspace |
| /workspaces/:id/members | GET | workspaceId | Danh sách member | Members + role trong workspace |

**Luồng tạo Workspace:**
1. Xác thực user qua JWT cookie
2. Kiểm tra name không trống (HTTP 400 nếu thiếu)
3. Tạo workspace, set `created_by` = user hiện tại
4. Tự động thêm user vào `workspace_members` với role ADMIN
5. Trả về workspaceId, name, ownerId

---

## 4. Module Workspace Member

**Mục đích:** Quản lý thành viên trong Workspace.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /workspaces-members/:id | POST | email (của user cần mời) | HTTP 200 | Thêm member vào workspace |

**Luồng mời member:**
1. Tìm user theo email qua `/api/v1/users?search=...`
2. Chọn user và gọi `POST /workspaces-members/:workspaceId`
3. Backend kiểm tra user tồn tại và chưa là member
4. Thêm vào `workspace_members` với role MEMBER
5. Cập nhật danh sách member trên UI

---

## 5. Module Board

**Mục đích:** Quản lý bảng công việc trong Workspace.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /boards | POST | workspaceId, title, backgroundUrl | Board mới | Tạo board trong workspace |
| /boards/:id | GET | boardId | Chi tiết board (kèm lists, cards) | Load toàn bộ board |
| /boards/:id | PUT | title | Board đã cập nhật | Cập nhật tên board |
| /boards/:id | DELETE | boardId | HTTP 204 | Xóa board |

**Logic kiểm tra quyền:**
- Workspace không tồn tại → HTTP 404 Not Found
- User không có quyền trong workspace → HTTP 403 Forbidden
- Board không tồn tại → HTTP 404 Not Found

**Background:** Chọn từ 8 ảnh nền cố định, URL lấy từ `GET /api/v1/images`

---

## 6. Module Board Member

**Mục đích:** Quản lý thành viên trong Board.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /boards-members | POST | userIds[], boardId, roleId, createdBy | HTTP 200 | Thêm member(s) vào board |
| /board-members?boardId=&search= | GET | boardId, search | Danh sách board member | Tìm kiếm member trong board |

**Lưu ý:** Chỉ user thuộc Workspace mới có thể được thêm vào Board.

---

## 7. Module List

**Mục đích:** Quản lý các cột (List) trong Board theo mô hình Kanban.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /lists | POST | boardId, title | List mới (listId, title, boardId) | Tạo list trong board |
| /lists/:id | PATCH | title, position | List đã cập nhật | Đổi tên hoặc di chuyển list |
| /lists/:id | DELETE | listId | HTTP 200 | Xóa list (cascade xóa cards bên trong) |
| /lists?boardId= | GET | boardId | Danh sách list có thứ tự | Lấy tất cả list, sắp theo position |

**Position:** Kiểu TEXT, cập nhật khi drag-and-drop để thay đổi thứ tự cột.

---

## 8. Module Card

**Mục đích:** Quản lý thẻ công việc (Card) trong List.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /cards | POST | listId, title, position | Card mới | Tạo card trong list |
| /cards/:id | GET | cardId | Chi tiết card | title, description, startDate, dueDate, position, isCompleted |
| /cards/:id | PATCH | title, description, startDate, dueDate, position, listId, isCompleted | Card cập nhật | Cập nhật thông tin hoặc di chuyển card |
| /cards/:id | DELETE | cardId | HTTP 200 | Xóa card |
| /cards?boardId= | GET | boardId | Danh sách card theo position | Tất cả cards thuộc board |

**Di chuyển Card (drag-and-drop):**
- PATCH /:id với `listId` (list đích) + `position` mới
- Hỗ trợ di chuyển card giữa các List khác nhau hoặc trong cùng List

**Luồng tạo Card:**
1. Xác thực user
2. Kiểm tra List tồn tại → HTTP 404 nếu không có
3. Kiểm tra quyền user trong board
4. Tạo card với `position` chỉ định
5. Trả về cardId, title, listId, position

---

## 9. Module Checklist

**Mục đích:** Quản lý danh sách kiểm tra (Checklist) bên trong Card.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /checklists | POST | cardId, name, position | Checklist mới | Tạo checklist trong card |
| /checklists/:id | PATCH | name, position | Checklist cập nhật | Đổi tên hoặc di chuyển |
| /checklists/:id | DELETE | checklistId | HTTP 200 | Xóa checklist (cascade xóa items) |
| /checklists?cardId= | GET | cardId | Danh sách checklist kèm items | Tất cả checklist thuộc card |

---

## 10. Module Checklist Item

**Mục đích:** Quản lý từng mục (item) trong Checklist.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /checklist-items | POST | checklistId, name, position, dueDate | Item mới (isCompleted=false) | Tạo item trong checklist |
| /checklist-items/:id | PATCH | name, position, dueDate, isCompleted | Item cập nhật | Cập nhật hoặc đánh dấu hoàn thành |
| /checklist-items/:id | DELETE | itemId | HTTP 200 | Xóa item |

**Trạng thái hoàn thành:** `isCompleted = true/false`. Frontend hiển thị Progress Bar tỷ lệ item đã hoàn thành.

---

## 11. Module Card Member

**Mục đích:** Gán/hủy thành viên được phụ trách cho Card.

| Endpoint | Method | Input | Output | Mô tả |
|---|---|---|---|---|
| /card-members | POST | userId, cardId | Card member mới | Gán thành viên vào card |
| /card-members/:id | DELETE | cardMemberId | HTTP 200 | Hủy gán thành viên khỏi card |

**Hiển thị:** Avatar thành viên hiển thị trực tiếp trên Card trong Kanban view.

---

## 12. Module Image

**Mục đích:** Cung cấp danh sách ảnh nền cho Board.

| Endpoint | Method | Output | Mô tả |
|---|---|---|---|
| /images | GET | 8 ảnh nền | Danh sách URL ảnh nền board |

---

## 13. Module WorkspaceCategory

**Mục đích:** Cung cấp danh sách phân loại Workspace.

| Endpoint | Method | Output | Mô tả |
|---|---|---|---|
| /workspace-categories | GET | 8 danh mục | Sales CRM, Education, HR, Operation, Marketing, IT, Small Business, Other |

---

## 14. Module Role

**Mục đích:** Cung cấp danh sách vai trò trong hệ thống.

| Endpoint | Method | Output | Mô tả |
|---|---|---|---|
| /roles | GET | 3 vai trò | ADMIN, MEMBER, VIEWER |

---

## Phân quyền theo module

| Thao tác | ADMIN | MEMBER | VIEWER |
|---|---|---|---|
| Tạo/xóa Workspace | ✓ | — | — |
| Mời Member vào Workspace | ✓ | — | — |
| Tạo/xóa Board | ✓ | — | — |
| Tạo/cập nhật/xóa List | ✓ | ✓ | — |
| Tạo/cập nhật/xóa Card | ✓ | ✓ | — |
| Thêm/xóa Checklist & Item | ✓ | ✓ | — |
| Gán/hủy Card Member | ✓ | ✓ | — |
| Xem Board/Card | ✓ | ✓ | ✓ |

*Lưu ý: Phân quyền VIEWER chưa được kiểm soát đầy đủ ở cả Frontend và Backend (W8)*

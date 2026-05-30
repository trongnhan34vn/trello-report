# Tuần 7 — 11/05/2026 – 17/05/2026

## Mục tiêu tuần 7
- Workspace Detail UI và Member Management
- Board UI theo mô hình Kanban (List + Card)
- Drag and Drop cho List và Card
- Tích hợp đầy đủ CRUD API cho List và Card

## Công việc đã thực hiện

### 1. Workspace Detail & Member Management

**Màn hình Workspace Detail (`/workspaces/:id`):**
- Hiển thị thông tin Workspace: name, description
- Hiển thị danh sách Board thuộc Workspace (dạng card có thumbnail ảnh nền)
- Hiển thị danh sách Member trong Workspace (kèm role)
- Tích hợp API:
  - `GET /api/v1/workspaces/:id` — thông tin workspace
  - `GET /api/v1/workspaces/:id/boards` — danh sách board
  - `GET /api/v1/workspaces/:id/members` — danh sách member

**Quản lý Member:**
- UI form mời member bằng email (modal "Invite Member")
- Tích hợp API `POST /api/v1/workspaces-members/:id` (body: email)
- Xử lý trạng thái loading và error khi gửi request
- Member mới xuất hiện ngay trong danh sách sau khi mời thành công

**Test cases:**
| TC | Kịch bản | Expected |
|---|---|---|
| TC01 | Load Workspace Detail thành công | HTTP 200, hiển thị đầy đủ Workspace, Board, Member |
| TC02 | Mời Member thành công | HTTP 200, member xuất hiện trong danh sách |

### 2. Board UI — Kanban Layout (`/boards/:id`)

**Màn hình Board:**
- Hiển thị danh sách List theo dạng cột (Kanban columns)
- Hiển thị Card bên trong từng List
- Tích hợp API `GET /api/v1/boards/:id` để load Board
- Xử lý empty state khi Board chưa có List hoặc List chưa có Card

**CRUD List trực tiếp trên Board:**
- Tạo List mới: `POST /api/v1/lists` (Input: boardId, title)
- Cập nhật tên List: `PATCH /api/v1/lists/:id`
- Xóa List: `DELETE /api/v1/lists/:id` (modal xác nhận)

**CRUD Card trực tiếp trên Board:**
- Tạo Card mới: `POST /api/v1/cards` (Input: listId, title)
- Xem chi tiết Card: Modal với đầy đủ thông tin
- Cập nhật Card: `PATCH /api/v1/cards/:id`
- Xóa Card: `DELETE /api/v1/cards/:id` (modal xác nhận)

**Test cases:**
| TC | Kịch bản | Expected |
|---|---|---|
| TC04 | Load Board thành công | HTTP 200, hiển thị đầy đủ List và Card |
| TC05 | Tạo List thành công | HTTP 201, List mới xuất hiện cuối danh sách cột |
| TC06 | Tạo Card thành công | HTTP 201, Card mới xuất hiện trong List tương ứng |
| TC07 | Xóa Card thành công | HTTP 200, Card biến mất khỏi Board |
| TC08 | Xóa List thành công | HTTP 200, List biến mất khỏi Board |

### 3. Drag and Drop (List & Card)

**Triển khai Drag-and-Drop:**
- Kéo thả List giữa các vị trí trong Board
- Kéo thả Card giữa các List khác nhau và trong cùng một List
- Cập nhật state UI ngay khi thao tác (optimistic update)
- Gửi request cập nhật position và listId lên backend sau khi drop:
  - List: `PATCH /api/v1/lists/:id` với `position` mới
  - Card: `PATCH /api/v1/cards/:id` với `listId` + `position` mới

**Kết quả mong muốn:**
- Drag-and-drop hoạt động mượt trên cả UI và backend
- Dữ liệu được đồng bộ chính xác sau mỗi thao tác kéo thả
- Trải nghiệm người dùng liền mạch, không reload trang

## Kết quả
- Hoàn thiện Workspace Detail (Board list + Member list + Invite Member)
- Board Kanban hoạt động đầy đủ với CRUD List và Card
- Drag-and-Drop cho List và Card được triển khai với optimistic update

## Kế hoạch tuần 8
- Hoàn thiện Board Member Management
- Card Detail Enhancement: Due Date, Checklist, Card Member
- Kiểm thử toàn hệ thống
- Chuẩn bị triển khai Staging

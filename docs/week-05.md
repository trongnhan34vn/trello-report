# Tuần 5 — 27/04/2026 – 03/05/2026

## Mục tiêu tuần 5
- Hoàn thiện API CRUD cho Card
- Triển khai Checklist và ChecklistItem trong Card
- Chuẩn hóa toàn bộ hệ thống API

## Công việc đã thực hiện

### 1. Quản lý Card (`/api/v1/cards`)

**Model Card:**
- Fields: `id`, `title`, `description`, `startDate`, `dueDate`, `isCompleted`, `position`, `listId`
- Quan hệ: Card thuộc List (`@ManyToOne`); thành viên gán qua bảng trung gian `CardMember`

**API đã triển khai:**

| API | Method | Input | Output |
|---|---|---|---|
| POST / | Tạo card | listId, title, position | cardId, title, listId, position |
| GET /:id | Lấy chi tiết | cardId | Đầy đủ: title, description, startDate, dueDate, position, isCompleted |
| PATCH /:id | Cập nhật | title, description, startDate, dueDate, position, listId, isCompleted | Thông tin card sau cập nhật |
| DELETE /:id | Xóa card | cardId | HTTP 200 |
| GET ?boardId= | Danh sách theo board | boardId | Danh sách card sắp xếp theo position |

**Di chuyển Card (drag-and-drop):**
- PATCH /:id với `listId` (list đích) + `position` (vị trí mới)
- Hỗ trợ di chuyển card giữa các list khác nhau

**Test cases TC17–TC22:**
| TC | Kịch bản | Expected |
|---|---|---|
| TC17 | Tạo card thành công | HTTP 200, cardId, title, listId, position |
| TC18 | Lấy chi tiết card thành công | HTTP 200, đầy đủ thông tin |
| TC19 | Cập nhật thông tin card | HTTP 200, thông tin được cập nhật |
| TC20 | Di chuyển card giữa list (drag-and-drop) | HTTP 200, listId và position cập nhật đúng |
| TC21 | Xóa card thành công | HTTP 200 |
| TC22 | Lấy danh sách card theo board | HTTP 200, sắp xếp theo position (hoặc []) |

### 2. Quản lý Checklist (`/api/v1/checklists`)

**Model Checklist:**
- Fields: `id`, `name`, `cardId`, `position`, `createdBy`, `updatedBy`
- Quan hệ: Checklist thuộc Card; ChecklistItem thuộc Checklist

**Model ChecklistItem:**
- Fields: `id`, `name`, `checklistId`, `position`, `dueDate`, `isCompleted`, `createdBy`, `updatedBy`

**API Checklist đã triển khai:**

| API | Method | Input | Output |
|---|---|---|---|
| POST /checklists | Tạo checklist | cardId, name, position | checklistId, name, cardId, position |
| PATCH /checklists/:id | Cập nhật | name, position | Thông tin checklist |
| DELETE /checklists/:id | Xóa | checklistId | HTTP 200 |
| GET /checklists?cardId= | Danh sách | cardId | Danh sách checklist kèm items |

**API ChecklistItem đã triển khai:**

| API | Method | Input | Output |
|---|---|---|---|
| POST /checklist-items | Tạo item | checklistId, name, position, dueDate | item với isCompleted=false |
| PATCH /checklist-items/:id | Cập nhật | name, position, dueDate, isCompleted | Thông tin item |
| DELETE /checklist-items/:id | Xóa | itemId | HTTP 200 |

**Test cases TC23–TC25:**
| TC | Kịch bản | Expected |
|---|---|---|
| TC23 | Tạo checklist thành công | HTTP 200, checklistId, name, cardId, position |
| TC24 | Tạo checklist item thành công | HTTP 200, isCompleted=false |
| TC25 | Cập nhật trạng thái checklist item | HTTP 200, isCompleted=true |

### 3. Tối ưu hóa hệ thống API
- Rà soát và chuẩn hóa toàn bộ API (Workspace, Board, List, Card)
- Chuẩn hóa format request/response theo một cấu trúc thống nhất
- Tối ưu query và xử lý dữ liệu với PostgreSQL
- Kiểm tra tính nhất quán dữ liệu giữa các bảng liên quan
- Xử lý các edge case (xóa cascade, dữ liệu orphan)

## Kết quả
- Hoàn thiện đầy đủ API CRUD cho Card, bao gồm drag-and-drop (position)
- Triển khai hoàn chỉnh Checklist và ChecklistItem
- Hệ thống API backend đồng bộ và sẵn sàng cho frontend

## Kế hoạch tuần 6
- Bắt đầu phát triển Frontend (NextJS)
- Authentication UI: Sign Up, Confirm Sign Up, Sign In
- Dashboard UI: Workspace và Board list

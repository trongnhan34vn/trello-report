# Tuần 4 — 20/04/2026 – 26/04/2026

## Mục tiêu tuần 4
- Phát triển chức năng quản lý Workspace và Board
- Xây dựng API cho List (tạo, cập nhật, xóa)
- Kiểm thử backend

## Công việc đã thực hiện

### 1. Quản lý Workspace (`/api/v1/workspaces`)

**API đã triển khai:**
- `POST /`: Tạo workspace mới (owner = user hiện tại). Input: `name`. Xác thực access token.
- `GET /`: Lấy danh sách workspace thuộc user hiện tại (trả về [] nếu chưa có)

**Validation:**
- `name` không được để trống → HTTP 400 nếu thiếu

**Cơ chế xác thực:**
- Kiểm tra access token trong cookie
- Từ chối request nếu token không hợp lệ hoặc hết hạn → HTTP 401

**Test cases TC01–TC05:**
| TC | Kịch bản | Expected |
|---|---|---|
| TC01 | Tạo workspace thành công | HTTP 201, trả về workspaceId, name, ownerId |
| TC02 | Thiếu tên workspace | HTTP 400 "Name is required" |
| TC03 | Token không hợp lệ | HTTP 401 Unauthorized |
| TC04 | Lấy danh sách workspace thành công | HTTP 200, danh sách workspace |
| TC05 | User chưa có workspace | HTTP 200, danh sách rỗng |

### 2. Quản lý Board (`/api/v1/boards`)

**API đã triển khai:**
- `POST /`: Tạo board trong workspace. Input: `workspaceId`, `title`, `backgroundUrl`
- `PUT /:id`: Cập nhật board (title) → HTTP 200
- `DELETE /:id`: Xóa board (kiểm tra quyền) → HTTP 204

**Logic nghiệp vụ:**
- Kiểm tra workspace tồn tại → HTTP 404 nếu không tìm thấy
- Kiểm tra quyền truy cập user trong workspace → HTTP 403 nếu không có quyền
- Tạo board và liên kết với workspace

**Test cases TC06–TC12:**
| TC | Kịch bản | Expected |
|---|---|---|
| TC06 | Tạo board thành công | HTTP 201, trả về boardId, title, workspaceId |
| TC07 | Workspace không tồn tại | HTTP 404 Not Found |
| TC08 | User không có quyền | HTTP 403 Forbidden |
| TC09 | Cập nhật board thành công | HTTP 200, title được cập nhật |
| TC10 | Board không tồn tại | HTTP 404 Not Found |
| TC11 | Xóa board thành công | HTTP 204 No Content |
| TC12 | Không có quyền xóa | HTTP 403 Forbidden |

### 3. Quản lý List (`/api/v1/lists`)

**API đã triển khai:**
- `POST /`: Tạo list trong board. Input: `boardId`, `title`. Trả về: `listId`, `title`, `boardId`
- `PUT /:id`: Cập nhật list (title, position). Input: `listId`, `title`
- `GET ?boardId=`: Lấy danh sách list theo board (kèm thứ tự hiển thị)

**Logic nghiệp vụ:**
- Kiểm tra board tồn tại trước khi tạo list → HTTP 404 nếu không có
- Xác thực người dùng trước mỗi thao tác
- Xử lý logic sắp xếp thứ tự List trong Board (position)
- Hỗ trợ trường hợp board chưa có list (trả về [])

**Test cases TC13–TC16:**
| TC | Kịch bản | Expected |
|---|---|---|
| TC13 | Tạo list thành công | HTTP 201, listId, title, boardId |
| TC14 | Board không tồn tại | HTTP 404 Not Found |
| TC15 | Cập nhật list thành công | HTTP 200, title "In Progress" |
| TC16 | Lấy danh sách list theo board | HTTP 200, danh sách list (hoặc []) |

## Kết quả
- Hoàn thành 6/9 API nhóm Workspace, Board và List
- Backend đạt nền tảng cho các chức năng quản lý dữ liệu cốt lõi
- Sẵn sàng cho phát triển chức năng Card

## Kế hoạch tuần 5
- Hoàn thiện API CRUD cho Card (tạo, cập nhật, xóa)
- Triển khai các thuộc tính mở rộng: description, assignee, due date
- Xây dựng logic xử lý position để phục vụ drag-and-drop
- Bắt đầu triển khai API Checklist trong Card

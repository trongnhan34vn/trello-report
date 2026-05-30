# Tuần 8 — 18/05/2026 – 24/05/2026

## Mục tiêu tuần 8
- Hoàn thiện Board Member Management
- Card Detail Enhancement: Due Date, Checklist, Card Member
- Kiểm thử toàn hệ thống
- Chuẩn bị triển khai Staging

## Công việc đã thực hiện

### 1. Board Member Management

**Tính năng:**
- Hiển thị danh sách thành viên hiện tại của Board (kèm role: ADMIN, MEMBER)
- Thêm thành viên từ danh sách thành viên thuộc Workspace

**Tích hợp API:**
```
GET  /api/v1/board-members?boardId=...&search=...
POST /api/v1/boards-members
BODY: {
  "userIds": ["string"],
  "boardId": "string",
  "roleId": 1073741824,
  "createdBy": "string"
}
```

- Đồng bộ dữ liệu Board sau khi thêm thành viên thành công
- Xử lý trạng thái loading và thông báo lỗi khi request thất bại

### 2. Card Detail Enhancement

#### 2.1 Due Date
- Lựa chọn và cập nhật Due Date bằng Date Picker trong Card Detail Modal
- Hiển thị trạng thái Due Date trực tiếp trên Card (badge: upcoming/overdue)
- Tích hợp API `PATCH /api/v1/cards/:id` với body:
  ```json
  {
    "startDate": "string",
    "dueDate": "string",
    "isCompleted": true
  }
  ```

#### 2.2 Checklist Management
- Hoàn thiện đầy đủ chức năng quản lý Checklist trong Card Detail Modal
- Hiển thị Progress Bar tiến độ hoàn thành checklist

**API đã tích hợp:**
```
POST   /api/v1/checklists
PATCH  /api/v1/checklists/:id
DELETE /api/v1/checklists/:id
POST   /api/v1/checklist-items
PATCH  /api/v1/checklist-items/:id
DELETE /api/v1/checklist-items/:id
```

**Tính năng hỗ trợ:**
- Tạo Checklist mới
- Cập nhật tên Checklist
- Xóa Checklist
- Thêm/xóa Checklist Item
- Đánh dấu hoàn thành / chưa hoàn thành (is_completed)
- Hiển thị tiến độ bằng Progress Bar

#### 2.3 Card Member Management
- Hiển thị danh sách thành viên được gán cho Card trong Card Detail Modal
- Hiển thị avatar thành viên trực tiếp trên Card (Kanban view)
- Tìm kiếm và thêm thành viên từ danh sách Board Members

**API đã tích hợp:**
```
POST   /api/v1/card-members
DELETE /api/v1/card-members/:id
```

### 3. Kiểm thử toàn hệ thống

Thực hiện kiểm thử tổng thể các luồng chức năng chính:
- Luồng đăng ký → xác nhận → đăng nhập → Dashboard
- Luồng tạo Workspace → tạo Board → tạo List → tạo Card
- Luồng Drag-and-Drop List và Card
- Luồng Card Detail: cập nhật Due Date, thêm Checklist, gán Member
- Ghi nhận các lỗi còn tồn tại

## Kết quả

### Đã hoàn thành
- Board Member Management UI hoạt động đầy đủ
- Due Date: thiết lập và hiển thị trạng thái trên Card
- Checklist: CRUD đầy đủ + Progress Bar
- Card Member: gán/hủy thành viên, hiển thị avatar
- Tích hợp hoàn chỉnh với backend API

### Vấn đề còn tồn tại

| Vấn đề | Mô tả |
|---|---|
| Drag-and-drop bug | Lỗi khi kéo vào vị trí đầu/cuối danh sách; sai lệch position giữa Frontend và Backend; cần rà soát thuật toán beforeId/afterId |
| Realtime Websocket | Chưa triển khai; các thay đổi từ user khác không cập nhật real-time; cần Websocket để đồng bộ Card, Checklist, Drag-and-Drop |
| VIEWER role | Phân quyền chưa hoàn thiện; hệ thống chưa giới hạn đầy đủ thao tác chỉnh sửa cho role VIEWER ở cả Frontend và Backend |
| Staging deployment | Chưa hoàn tất; vấn đề đồng nhất domain Frontend/Backend để hỗ trợ HttpOnly Cookie cross-domain |

## Hướng phát triển tiếp theo (sau W8)

1. **Bug Drag and Drop:** Rà soát thuật toán xác định beforeId/afterId, sửa cơ chế cập nhật position
2. **Realtime Websocket:** Đồng bộ sự kiện tạo Card, cập nhật Card, thay đổi trạng thái Checklist, Drag-and-Drop
3. **Phân quyền VIEWER:** Bổ sung kiểm soát quyền ở cả Frontend và Backend
4. **Quản lý nhãn dán (Tag):** Tạo, gắn, tìm kiếm và lọc Card theo Tag
5. **Hệ thống thông báo (Notification):** Thông báo khi Card tạo/cập nhật, khi thành viên được gán, khi công việc sắp đến hạn
6. **Nhật ký hoạt động (Activity Log):** Ghi nhận lịch sử tạo, cập nhật, xóa dữ liệu
7. **Triển khai Staging:** Xử lý vấn đề HttpOnly Cookie cross-domain

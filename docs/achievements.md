# Kết quả đạt được

## Tổng kết

| Hạng mục | Kết quả |
|---|---|
| Thời gian thực hiện | 8 tuần (30/03 – 24/05/2026) |
| Số API endpoint hoàn thành | 14 nhóm (toàn bộ backend) |
| Số màn hình Frontend | 6 màn hình chính |
| Số test cases đã kiểm thử | > 50 test cases |
| Trạng thái backend | Hoàn chỉnh |
| Trạng thái frontend | Hoàn chỉnh các chức năng cốt lõi |
| Triển khai | Local development (Staging chưa hoàn tất) |

---

## Backend — Chức năng hoàn thành

### Module Authentication
- [x] Đăng ký tài khoản qua AWS Cognito (POST /auth/sign-up)
- [x] Xác nhận email bằng OTP (POST /auth/confirm-sign-up)
- [x] Đăng nhập và nhận JWT (POST /auth/sign-in)
- [x] Làm mới access token (POST /auth/refresh-token)
- [x] Gửi lại mã xác nhận (POST /auth/resend-code)
- [x] Token lưu trong HttpOnly Cookie

### Module User
- [x] Tìm kiếm user theo email hoặc tên (GET /users)
- [x] Lấy thông tin user hiện tại (GET /users/me)

### Module Workspace
- [x] Tạo workspace (POST /workspaces)
- [x] Lấy danh sách workspace của user (GET /workspaces)
- [x] Lấy chi tiết workspace (GET /workspaces/:id)
- [x] Lấy danh sách board thuộc workspace (GET /workspaces/:id/boards)
- [x] Lấy danh sách member trong workspace (GET /workspaces/:id/members)
- [x] Mời member vào workspace (POST /workspaces-members/:id)

### Module Board
- [x] Tạo board trong workspace (POST /boards)
- [x] Lấy chi tiết board kèm lists/cards (GET /boards/:id)
- [x] Cập nhật board (PUT /boards/:id)
- [x] Xóa board (DELETE /boards/:id)
- [x] Thêm member vào board (POST /boards-members)
- [x] Tìm kiếm member trong board (GET /board-members)

### Module List
- [x] Tạo list trong board (POST /lists)
- [x] Cập nhật list (PATCH /lists/:id)
- [x] Xóa list (DELETE /lists/:id)
- [x] Lấy danh sách list theo board (GET /lists?boardId=)

### Module Card
- [x] Tạo card trong list (POST /cards)
- [x] Lấy chi tiết card (GET /cards/:id)
- [x] Cập nhật card — title, description, startDate, dueDate, isCompleted (PATCH /cards/:id)
- [x] Di chuyển card giữa list (PATCH /cards/:id với listId + position)
- [x] Xóa card (DELETE /cards/:id)
- [x] Lấy danh sách card theo board (GET /cards?boardId=)

### Module Checklist & ChecklistItem
- [x] Tạo / cập nhật / xóa checklist (POST, PATCH, DELETE /checklists)
- [x] Lấy danh sách checklist theo card (GET /checklists?cardId=)
- [x] Tạo / cập nhật / xóa checklist item (POST, PATCH, DELETE /checklist-items)
- [x] Cập nhật trạng thái hoàn thành (isCompleted)

### Module Card Member
- [x] Gán thành viên vào card (POST /card-members)
- [x] Hủy gán thành viên (DELETE /card-members/:id)

### Module Phụ trợ
- [x] Danh sách ảnh nền board (GET /images) — 8 ảnh
- [x] Danh sách workspace category (GET /workspace-categories) — 8 loại
- [x] Danh sách role (GET /roles) — ADMIN, MEMBER, VIEWER

---

## Frontend — Màn hình và chức năng hoàn thành

### Màn hình Sign Up (`/sign-up`)
- Form nhập fullName, email, password, confirmPassword
- Validation phía client
- Thông báo lỗi khi user đã tồn tại ("User already exists")
- Redirect sang Confirm Sign Up sau khi đăng ký

### Màn hình Confirm Sign Up (`/confirm-sign-up`)
- Nhập mã OTP 6 chữ số
- Nút "Let's re-send code" (có countdown timer)
- Thông báo lỗi khi mã sai ("Invalid verification code")
- Redirect sang Sign In sau khi xác nhận

### Màn hình Sign In (`/sign-in`)
- Form email + password
- Thông báo lỗi khi sai thông tin ("Invalid Credentials")
- Lưu token vào HttpOnly Cookie
- Redirect sang Dashboard

### Dashboard
- Hiển thị danh sách Workspace của user (sidebar)
- Hiển thị Board theo từng Workspace (thumbnail có ảnh nền)
- Xử lý empty state khi chưa có workspace/board
- Auto refresh token khi session hết hạn
- Nút "Create New Workspace" và "Create Board"

### Workspace Detail (`/workspaces/:id`)
- Thông tin workspace (name, description)
- Danh sách Board thuộc workspace (card có ảnh nền)
- Danh sách Member với role (ADMIN/MEMBER)
- Modal mời member bằng email
- Xử lý loading/error state

### Board Kanban (`/boards/:id`)
- Layout Kanban: danh sách List theo dạng cột ngang
- Hiển thị Card bên trong từng List
- Tạo List mới (form inline)
- Đổi tên List (click để edit)
- Xóa List (modal xác nhận)
- Tạo Card mới (form inline trong List)
- Xóa Card (modal xác nhận)
- Card Detail Modal: title, description, Due Date, Checklist, Members
- Drag and Drop: kéo thả List, kéo thả Card giữa các List
- Board Member Management: xem + thêm member vào board
- Due Date badge trên Card (overdue/upcoming)
- Avatar thành viên trên Card

---

## Kết quả kiểm thử theo module

### Authentication (W3) — 14 test cases
| TC | Kịch bản | Kết quả |
|---|---|---|
| TC01–03 | Sign Up: hợp lệ, trùng email, thiếu field | Pass |
| TC04–05 | Confirm Sign Up: mã đúng, mã sai | Pass |
| TC06–09 | Sign In: hợp lệ, sai pw, chưa confirm, cookie | Pass |
| TC10–12 | Refresh Token: hợp lệ, thiếu token, hết hạn | Pass |
| TC13–14 | Resend Code: user hợp lệ, không tồn tại | Pass |

### Workspace & Board (W4) — 16 test cases
| TC | Kịch bản | Kết quả |
|---|---|---|
| TC01–05 | Workspace: tạo, thiếu tên, token sai, lấy ds, ds rỗng | Pass |
| TC06–08 | Board: tạo, workspace không tồn tại, không quyền | Pass |
| TC09–12 | Board: cập nhật, không tồn tại, xóa, không quyền | Pass |
| TC13–16 | List: tạo, board không tồn tại, cập nhật, lấy ds | Pass |

### Card & Checklist (W5) — 9 test cases
| TC | Kịch bản | Kết quả |
|---|---|---|
| TC17–22 | Card: tạo, lấy chi tiết, cập nhật, drag-drop, xóa, ds theo board | Pass |
| TC23–25 | Checklist: tạo, tạo item, cập nhật trạng thái item | Pass |

### Frontend Integration (W6) — 9 test cases
| TC | Kịch bản | Kết quả |
|---|---|---|
| TC01–03 | Auth flow: đăng ký → OTP → đăng nhập | Pass |
| TC04–06 | Auth lỗi: user tồn tại, sai mật khẩu, OTP sai | Pass |
| TC07–09 | Dashboard: load, tạo workspace, tạo board | Pass |

### Board UI (W7) — 8 test cases
| TC | Kịch bản | Kết quả |
|---|---|---|
| TC01–02 | Workspace Detail: load, mời member | Pass |
| TC04–08 | Board: load, tạo list, tạo card, xóa card, xóa list | Pass |

---

## Đánh giá kết quả

### Ưu điểm
- **Kiến trúc rõ ràng:** Frontend và Backend tách biệt hoàn toàn, giao tiếp qua REST API chuẩn hóa
- **Bảo mật:** Sử dụng AWS Cognito (dịch vụ chuyên dụng) và HttpOnly Cookie (chống XSS)
- **Trải nghiệm UX:** Giao diện Kanban trực quan, hỗ trợ drag-and-drop với optimistic update
- **Codebase sạch:** DTO + Validation, Exception Handler tập trung, chuẩn hóa response format
- **Kiểm thử kỹ:** Hơn 50 test cases bao phủ các luồng thành công và thất bại
- **Database thiết kế tốt:** Cascade delete, unique constraint, position dạng TEXT linh hoạt

### Hạn chế
- Chưa triển khai được lên môi trường Staging
- Drag-and-drop còn lỗi edge case (vị trí đầu/cuối danh sách)
- Phân quyền VIEWER chưa kiểm soát đầy đủ
- Chưa có Realtime (Websocket)

# Hạn chế và hướng phát triển

## Các vấn đề còn tồn tại (tính đến W8 — 24/05/2026)

### 1. Lỗi Drag and Drop — vị trí đầu/cuối danh sách

**Mô tả:**  
Thuật toán xác định vị trí (position) khi kéo thả List hoặc Card vào đầu hoặc cuối danh sách chưa hoạt động chính xác. Khi di chuyển Card giữa các List, có trường hợp vị trí hiển thị trên Frontend không khớp với dữ liệu lưu trong Backend.

**Nguyên nhân:**  
Thuật toán tính `beforeId`/`afterId` và cập nhật position dạng chuỗi (string-based) chưa xử lý hết các trường hợp biên (edge case): kéo vào vị trí đầu tiên (không có `beforeId`), kéo vào vị trí cuối cùng (không có `afterId`).

**Ảnh hưởng:**  
Thứ tự hiển thị List/Card có thể sai lệch sau một số thao tác kéo thả liên tiếp.

---

### 2. Chưa có Realtime Websocket

**Mô tả:**  
Hệ thống hiện tại đồng bộ dữ liệu hoàn toàn thông qua API request theo mô hình request-response. Khi một người dùng thực hiện thao tác (tạo Card, di chuyển Card, cập nhật Checklist), những người dùng khác đang xem cùng Board không được thông báo theo thời gian thực.

**Tác động:**  
Người dùng phải reload trang thủ công để thấy thay đổi từ thành viên khác. Điều này ảnh hưởng đến trải nghiệm cộng tác thời gian thực.

**Hướng giải quyết:**  
Triển khai Websocket (ví dụ: Spring WebSocket + STOMP) để đẩy sự kiện từ server xuống client khi có thay đổi dữ liệu:
- Sự kiện tạo/cập nhật/xóa Card
- Sự kiện thay đổi trạng thái Checklist Item
- Sự kiện Drag and Drop

---

### 3. Phân quyền VIEWER chưa hoàn thiện

**Mô tả:**  
Role VIEWER được thiết kế để chỉ có quyền xem, không thể thực hiện thao tác chỉnh sửa. Tuy nhiên, hệ thống hiện tại chưa giới hạn đầy đủ các thao tác cho vai trò này ở cả phía Frontend (UI) và Backend (API).

**Tác động:**  
Người dùng có role VIEWER có thể thực hiện một số thao tác mà lẽ ra không được phép.

**Hướng giải quyết:**
- Backend: bổ sung kiểm tra role trong từng API endpoint liên quan đến write operations
- Frontend: ẩn/vô hiệu hóa các nút thao tác khi user có role VIEWER

---

### 4. Triển khai Staging chưa hoàn tất

**Mô tả:**  
Hệ thống hiện chỉ hoạt động trên môi trường phát triển cục bộ (localhost). Việc triển khai lên môi trường Staging (server công khai) chưa hoàn tất.

**Vấn đề kỹ thuật cụ thể:**  
HttpOnly Cookie yêu cầu Frontend và Backend phải cùng domain hoặc được cấu hình CORS đúng cách. Khi triển khai lên các domain khác nhau (ví dụ: `nello.vercel.app` cho Frontend, `api.nello.com` cho Backend), cookie cross-domain không hoạt động với thiết lập mặc định.

**Hướng giải quyết:**
- Cấu hình `SameSite=None; Secure` cho cookie
- Đặt cả Frontend và Backend cùng subdomain của một domain chính
- Hoặc chuyển sang lưu token trong `Authorization` header thay vì cookie (thay đổi kiến trúc)

---

## Chức năng chưa được triển khai

| Tính năng | Mô tả | Lý do chưa làm |
|---|---|---|
| Quản lý nhãn dán (Tag) | Tạo, gắn, tìm kiếm và lọc Card theo Tag | Ngoài phạm vi 8 tuần |
| Hệ thống thông báo (Notification) | Thông báo khi Card được tạo/cập nhật, khi được gán thành viên, khi gần deadline | Ngoài phạm vi 8 tuần |
| Nhật ký hoạt động (Activity Log) | Ghi nhận lịch sử tạo, cập nhật, xóa dữ liệu; lịch sử thay đổi Board/List/Card | Ngoài phạm vi 8 tuần |
| Tìm kiếm toàn cục | Tìm kiếm Card theo tên, mô tả, tag trên toàn Board/Workspace | Ngoài phạm vi 8 tuần |
| Xem lịch (Calendar view) | Hiển thị Card theo due date trên lịch | Ngoài phạm vi 8 tuần |
| Upload file đính kèm | Đính kèm file vào Card | Ngoài phạm vi 8 tuần |
| Realtime Websocket | Đồng bộ thời gian thực giữa các user | Đã nhận diện, chưa triển khai |

---

## Hướng phát triển tiếp theo

### Ưu tiên cao (cần giải quyết sớm)

**1. Sửa bug Drag and Drop**
- Rà soát lại thuật toán tính beforeId/afterId
- Kiểm tra các trường hợp biên: kéo vào đầu list, cuối list, list rỗng
- Đảm bảo đồng bộ position giữa Frontend state và Backend database

**2. Hoàn thiện phân quyền VIEWER**
- Backend: middleware kiểm tra role trước write operations
- Frontend: kiểm tra role từ API `/users/me` và ẩn UI tương ứng

**3. Giải quyết vấn đề Staging deployment**
- Lựa chọn chiến lược cookie/token phù hợp với cross-domain
- Cấu hình CORS và cookie policy cho môi trường production

### Ưu tiên trung bình (mở rộng tính năng)

**4. Realtime Websocket**
- Tích hợp Spring WebSocket (STOMP) vào backend
- Tích hợp SockJS/STOMP client vào NextJS frontend
- Đẩy sự kiện: Card CRUD, Checklist changes, Drag-and-Drop

**5. Quản lý nhãn dán (Tag)**
- Tạo, chỉnh sửa và xóa Tag (màu sắc, tên)
- Gắn nhiều Tag cho một Card
- Tìm kiếm và lọc Card theo Tag trong Board

**6. Hệ thống thông báo (Notification)**
- Thông báo khi Card được giao cho user
- Thông báo khi công việc sắp đến hạn hoặc đã quá hạn
- Thông báo khi thành viên được mời vào Workspace/Board

### Ưu tiên thấp (phát triển dài hạn)

**7. Nhật ký hoạt động (Activity Log)**
- Ghi nhận lịch sử thay đổi: ai tạo, ai cập nhật, ai xóa
- Hiển thị timeline hoạt động trong Card Detail
- Theo dõi lịch sử thay đổi của Board

**8. Tính năng tìm kiếm**
- Tìm kiếm Card theo tiêu đề, mô tả
- Tìm kiếm trong toàn Workspace

**9. Các view thay thế**
- Calendar View: hiển thị Card theo due date
- Timeline View: hiển thị công việc theo tiến trình thời gian

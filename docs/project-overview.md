# Tổng quan dự án

## Thông tin chung

| Mục | Nội dung |
|---|---|
| Tên đề tài | Hệ thống Quản lý Công việc |
| Tên ứng dụng | Nello |
| Sinh viên | Nguyễn Trọng Nhân – 202490073 |
| Lớp | CNTT 1.1 – K69 |
| Giảng viên hướng dẫn | Th.S Phạm Quang Hiếu |
| Trường | Đại học Bách Khoa Hà Nội – SOICT |
| Thời gian | 30/03/2026 – 24/05/2026 (8 tuần) |

---

## Bài toán cần giải quyết

Trong môi trường làm việc hiện đại, các nhóm cần một công cụ để:
- Tổ chức và theo dõi công việc theo từng giai đoạn (To Do → In Progress → Done)
- Phân công công việc cho từng thành viên
- Theo dõi hạn hoàn thành (due date) và tiến độ (checklist)
- Cộng tác nhiều người trên cùng một bảng công việc

**Phương pháp truyền thống** (bảng tính, email, ghi chú) có nhiều hạn chế:
- Khó theo dõi trạng thái từng công việc
- Không hỗ trợ cộng tác thời gian thực
- Thiếu phân quyền theo vai trò
- Không có cơ chế nhắc nhở deadline

---

## Mô tả hệ thống

Hệ thống quản lý công việc theo mô hình **Kanban**, lấy cảm hứng từ Trello. Người dùng có thể:

1. Tạo **Workspace** (không gian làm việc) để nhóm các dự án liên quan
2. Tạo **Board** (bảng công việc) trong Workspace, có ảnh nền tùy chọn
3. Tạo **List** (cột) trong Board (ví dụ: To Do, In Progress, Done)
4. Tạo **Card** (thẻ công việc) trong List với đầy đủ thông tin
5. Kéo thả Card và List để sắp xếp công việc (drag-and-drop)
6. Mời thành viên vào Workspace và Board với phân quyền ADMIN/MEMBER/VIEWER
7. Quản lý **Checklist** và **Checklist Item** trong Card
8. Thiết lập **Due Date** và theo dõi trạng thái hoàn thành

---

## Mục tiêu đề tài

**Mục tiêu chức năng:**
- Xây dựng hệ thống quản lý công việc theo mô hình Kanban đầy đủ
- Cung cấp giao diện trực quan, hỗ trợ thao tác kéo thả
- Tích hợp xác thực người dùng an toàn qua AWS Cognito
- Hỗ trợ phân quyền thành viên (ADMIN, MEMBER, VIEWER)

**Mục tiêu kỹ thuật:**
- Thiết kế và triển khai RESTful API chuẩn hóa với Spring Boot
- Sử dụng PostgreSQL trên AWS RDS để lưu trữ dữ liệu
- Xây dựng Frontend với NextJS, giao tiếp với backend qua API
- Bảo mật phiên làm việc bằng HttpOnly Cookie và JWT

---

## Phạm vi đề tài

**Trong phạm vi:**
- Backend: API quản lý Workspace, Board, List, Card, Checklist, thành viên
- Frontend: UI xác thực, Dashboard, Workspace Detail, Board Kanban, Card Detail
- Authentication: Đăng ký, xác nhận email, đăng nhập, refresh token
- Drag-and-drop cho List và Card trong Board

**Ngoài phạm vi:**
- Realtime Websocket (đồng bộ thời gian thực giữa các user)
- Hệ thống thông báo (Notification)
- Nhật ký hoạt động (Activity Log)
- Quản lý nhãn dán (Tag)
- Triển khai lên môi trường production (chỉ development local)

---

## Đối tượng sử dụng

| Đối tượng | Nhu cầu |
|---|---|
| Người quản lý dự án | Tạo và quản lý Workspace, Board; phân quyền thành viên |
| Thành viên nhóm | Tạo, cập nhật, di chuyển Card; quản lý Checklist |
| Người xem (Viewer) | Theo dõi tiến độ công việc (chỉ xem) |

---

## Tính cấp thiết

- **Nhu cầu thực tế:** Các nhóm làm việc cần công cụ quản lý công việc nhẹ, dễ sử dụng, không phụ thuộc phần mềm thương mại đắt tiền
- **Bối cảnh học thuật:** Đề tài giúp áp dụng toàn bộ quy trình phát triển phần mềm: phân tích, thiết kế, lập trình, kiểm thử
- **Giá trị kỹ thuật:** Tích hợp các dịch vụ cloud (AWS Cognito, AWS RDS), kiến trúc phân tách Frontend/Backend, RESTful API chuẩn hóa

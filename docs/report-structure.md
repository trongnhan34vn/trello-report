# Cấu trúc báo cáo — Nello Task Management

> Cập nhật: 2026-06-26  
> File nguồn: `src/content.tex` (3008 dòng)

---

## Tổng quan theo chương

| Chương | Tiêu đề | Dòng | Số dòng |
|--------|---------|------|---------|
| 1 | Giới thiệu đề tài | 1 – 53 | 53 |
| 2 | Cơ sở lý thuyết | 54 – 217 | 164 |
| 3 | Phân tích và thiết kế hệ thống | 218 – 2556 | **2338** |
| 4 | Triển khai và kết quả | 2557 – 2889 | 333 |
| 5 | Kết luận và hướng phát triển | 2890 – 3008 | 119 |

---

## Chi tiết Chương 3 (nặng nhất)

### 3.1 Phân tích hệ thống

| Mục | Tiêu đề | Dòng | Số dòng | Ghi chú |
|-----|---------|------|---------|---------|
| 3.1.1 | Khảo sát và xác định yêu cầu | 222 – 392 | 171 | Yêu cầu chức năng + phi chức năng |
| 3.1.2 | Xác định tác nhân và chức năng | 393 – 522 | 130 | Use Case Diagram tổng quan |
| 3.1.3 | Đặc tả Use Case | 523 – 1368 | **845** | ⚠️ Dài — 16 use case, mỗi cái có đủ flow |
| 3.1.4 | Phân tích luồng nghiệp vụ | 1369 – 1818 | **449** | 36 activity diagram (UC-01.1 → UC-08.2) |
| 3.1.5 | Phân tích dữ liệu | 1819 – 1845 | 27 | ERD |

### 3.2 Thiết kế hệ thống

| Mục | Tiêu đề | Dòng | Số dòng | Ghi chú |
|-----|---------|------|---------|---------|
| 3.2.1 | Thiết kế kiến trúc tổng thể | 1848 – 1902 | 55 | Component diagram + AWS deployment |
| 3.2.2 | Thiết kế tương tác | 1903 – 2036 | 134 | 10 sequence diagram (SD-01 → SD-10) |
| 3.2.3 | Thiết kế cơ sở dữ liệu | 2037 – 2550 | **514** | ⚠️ Dài — 13 bảng, mô tả cột chi tiết |
| ~~3.2.4~~ | ~~Thiết kế giao diện~~ | — | — | Đã xóa |

---

## Danh sách biểu đồ

### Use Case Diagram
| File | Nội dung |
|------|----------|
| `usecase_overview.png` | Tổng quan toàn bộ hệ thống |
| `usecase_uc01.png` – `usecase_uc08.png` | Chi tiết từng nhóm UC |

### Activity Diagram (36 biểu đồ)
| File | Nội dung |
|------|----------|
| `activity_uc01_1.png` – `activity_uc01_2.png` | UC-01: Đăng ký / Đăng nhập |
| `activity_uc02_1.png` – `activity_uc02_2.png` | UC-02: Workspace |
| `activity_uc03_1.png` – `activity_uc03_2.png` | UC-03: Board |
| `activity_uc04_1.png` – `activity_uc04_2.png` | UC-04: List |
| `activity_uc05_1.png` – `activity_uc05_2.png` | UC-05: Card |
| `activity_uc06_1.png` – `activity_uc06_2.png` | UC-06: Checklist |
| `activity_uc07_1.png` – `activity_uc07_2.png` | UC-07: Thành viên |
| `activity_uc08_1.png` – `activity_uc08_2.png` | UC-08: Thông báo / Cài đặt |

### Sequence Diagram (10 biểu đồ)
| SD | Luồng | File |
|----|-------|------|
| SD-01 | Đăng ký tài khoản (2 bước, OTP) | `sequence_sd01.png` |
| SD-02 | Đăng nhập (Cognito + lazy provisioning) | `sequence_sd02.png` |
| SD-03 | Tạo Workspace | `sequence_sd03.png` |
| SD-04 | Tạo Board (kiểm tra Workspace member) | `sequence_sd04.png` |
| SD-05 | Tạo List (fractional indexing) | `sequence_sd05.png` |
| SD-06 | Tạo Card (kiểm tra Board member) | `sequence_sd06.png` |
| SD-07 | Tải chi tiết Board (nested read) | `sequence_sd07.png` |
| SD-08 | Thêm thành viên vào Board | `sequence_sd08.png` |
| SD-09 | Di chuyển Card (optimistic update) | `sequence_sd09.png` |
| SD-10 | Di chuyển List (optimistic update) | `sequence_sd10.png` |

---

## Điểm cần chú ý

| # | Vấn đề | Mức độ | Gợi ý xử lý |
|---|--------|--------|-------------|
| 1 | 3.1.3 Đặc tả Use Case quá dài (845 dòng) | 🔴 Cao | Cắt bớt Alt Flow ở các UC ít quan trọng, chỉ giữ Main Flow |
| 2 | 3.2.3 Thiết kế CSDL dài (514 dòng) | 🟠 Trung bình | Gộp bảng nhỏ, bỏ cột mô tả nếu tên cột đã đủ rõ |
| 3 | Chương 3 chiếm ~78% toàn bộ báo cáo | 🟠 Trung bình | Cân nhắc rút gọn 3.1.3 và 3.2.3 |

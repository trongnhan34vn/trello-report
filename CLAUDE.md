# Hướng dẫn sinh nội dung báo cáo Project I

## Mục tiêu

Sinh nội dung báo cáo Project I dưới dạng LaTeX.

KHÔNG tạo:

- Trang bìa
- Lời cảm ơn
- Mục lục
- Danh mục hình ảnh
- Danh mục bảng biểu
- Phần cấu hình LaTeX
- Các package LaTeX

Chỉ sinh phần nội dung báo cáo.

---

## Nguồn dữ liệu

Ưu tiên sử dụng các nguồn sau:

1. Các file tổng hợp trong docs
2. Mã nguồn của dự án.
3. Các tài liệu thiết kế và kiến trúc hệ thống.
4. Hình ảnh, sơ đồ, tài liệu đi kèm trong repository.

Không tự suy diễn hoặc bổ sung thông tin không tồn tại trong tài liệu.

Nếu thiếu dữ liệu thì ghi:

\textbf{TODO: Bổ sung nội dung}

---

## Ngôn ngữ

Sử dụng tiếng Việt.

Văn phong phải mang tính học thuật.

Không sử dụng:

- Tôi
- Mình
- Chúng tôi
- Nhóm em
- Em
- Nhóm mình

Ưu tiên cách viết khách quan.

Ví dụ:

Sai:

"Tôi đã xây dựng chức năng đăng nhập."

Đúng:

"Chức năng đăng nhập đã được xây dựng."

---

## Trình bày

1. Các Chương thì cần được viết hoa toàn bộ. Ví dụ: CHƯƠNG 1: GIỚI THIỆU ĐỀ TÀI.
2. Các phần nhỏ hơn trong chương thì viết thường. Ví dụ: 1.1. Giới thiệu đề tài

## Đầu ra

Toàn bộ nội dung báo cáo phải được sinh vào file:

```text
src/content.tex
```

Không tạo thêm file `.tex` khác nếu không được yêu cầu.

Không chỉnh sửa:

- Trang bìa
- Mục lục
- File cấu hình LaTeX
- File main.tex

Chỉ cập nhật nội dung bên trong:

```text
src/content.tex
```

---

## Quy tắc sinh nội dung

File `src/content.tex` chỉ chứa nội dung báo cáo.

Không sinh:

```latex
\documentclass
\begin{document}
\end{document}
```

Không sinh:

- Trang bìa
- Mục lục
- Lời cảm ơn

Chỉ sinh phần nội dung chính của báo cáo.

---

## Cấu trúc thư mục

Dự án sử dụng cấu trúc:

```text
reports/
├── AGENTS.md
├── src/
│   └── content.tex
│   └── images/
│   └── cover.tex
│   └── main.tex
│   └── thanks.tex
├── weekly-reports/

```
---

## Quy trình làm việc

Trước khi viết báo cáo:

1. Đọc toàn bộ các file tổng hợp trong `docs/`, hoặc memory.
2. Tổng hợp các chức năng đã thực hiện.
3. Tổng hợp các công nghệ đã sử dụng.
4. Tổng hợp kiến trúc hệ thống.
5. Tổng hợp cơ sở dữ liệu.
6. Tổng hợp các kết quả đạt được.
7. Sử dụng các thông tin này để viết báo cáo.

Nếu nhiều báo cáo tuần chứa cùng một nội dung:

- Ưu tiên phiên bản mới nhất.
- Loại bỏ nội dung trùng lặp.
- Đảm bảo tính nhất quán giữa các chương.

---

## Quy trình cập nhật báo cáo

Khi được yêu cầu viết báo cáo:

1. Đọc các file PDF trong `docs/`, hoặc memory
2. Phân tích source code hiện có
3. Tổng hợp nội dung
4. Ghi kết quả vào:
   `src/content.tex`

Không tạo file output ở vị trí khác.

---

# Cấu trúc báo cáo

## Chương 1. Giới thiệu đề tài

Bao gồm:

### Giới thiệu đề tài

Trình bày:

- Bài toán cần giải quyết
- Bối cảnh thực tế
- Nhu cầu phát sinh

### Tính cấp thiết của đề tài

Trả lời rõ:

- Vì sao cần thực hiện đề tài
- Những hạn chế của phương pháp hiện tại
- Lợi ích mà hệ thống mang lại

### Mục tiêu đề tài

### Phạm vi đề tài

### Đối tượng sử dụng

---

## Chương 2. Cơ sở lý thuyết

Bao gồm:

### Cách tiếp cận và hướng giải quyết

Mô tả:

- Phương pháp phân tích bài toán
- Hướng thiết kế giải pháp
- Các quyết định kỹ thuật chính

### Các nền tảng và công nghệ sử dụng

Đối với mỗi công nghệ cần trình bày:

- Giới thiệu
- Chức năng
- Vai trò trong hệ thống

Ví dụ:

- ReactJS
- NextJS
- NestJS
- Spring Boot
- MySQL
- PostgreSQL
- Redis
- RabbitMQ
- Docker
- AWS
- JWT
- OAuth2
- Amazon Cognito

### Kiến trúc hệ thống

Mô tả kiến trúc đang áp dụng.

---

# Chương 3. Phân tích và thiết kế hệ thống

## 3.1. Phân tích hệ thống

### 3.1.1. Khảo sát và xác định yêu cầu

#### Mục đích

* Xác định hệ thống cần làm gì.
* Xác định phạm vi bài toán.

#### Nội dung

* Yêu cầu chức năng (Functional Requirements)
* Yêu cầu phi chức năng (Non-functional Requirements)

#### Biểu đồ

Không bắt buộc.

---

### 3.1.2. Xác định tác nhân và chức năng hệ thống

#### Mục đích

* Xác định ai sử dụng hệ thống.
* Xác định các chức năng hệ thống cung cấp.

#### Biểu đồ

**Use Case Diagram**

#### Trả lời câu hỏi

> Hệ thống phải làm gì?

#### Ví dụ

```text
User
 ├─ Create Workspace
 ├─ Create Board
 ├─ Create List
 ├─ Create Card
 └─ Assign Member
```

---

### 3.1.3. Đặc tả Use Case

#### Mục đích

Mô tả chi tiết từng Use Case.

#### Biểu đồ

Không bắt buộc.

#### Nội dung

* Actor
* Preconditions
* Main Flow
* Alternative Flow
* Postconditions

---

### 3.1.4. Phân tích luồng nghiệp vụ

#### Mục đích

Mô tả trình tự xử lý của một chức năng.

#### Biểu đồ

**Activity Diagram**

#### Trả lời câu hỏi

> Chức năng diễn ra như thế nào?

#### Ví dụ

```text
Create Card
    ↓
Validate Input
    ↓
Save Database
    ↓
Notify Success
```

---

### 3.1.5. Phân tích dữ liệu

#### Mục đích

Xác định dữ liệu cần lưu trữ.

#### Biểu đồ

**ERD (Entity Relationship Diagram)**

#### Trả lời câu hỏi

> Hệ thống quản lý những dữ liệu gì?

#### Ví dụ

```text
Workspace
Board
List
Card
User
```

---

## 3.2. Thiết kế hệ thống

### 3.2.1. Thiết kế kiến trúc tổng thể

#### Mục đích

Xác định các thành phần lớn của hệ thống.

#### Biểu đồ

**Component Diagram** hoặc **Architecture Diagram**

#### Trả lời câu hỏi

> Hệ thống được chia thành các module nào?

#### Ví dụ

```text
Client
   |
Server
   |
Database
```

---

### 3.2.2. Thiết kế tương tác giữa các thành phần

#### Mục đích

Mô tả cách các đối tượng giao tiếp với nhau.

#### Biểu đồ

**Sequence Diagram**

#### Trả lời câu hỏi

> Ai gọi ai và theo thứ tự nào?

#### Ví dụ

```text
User
  |
Controller
  |
Service
  |
Repository
  |
Database
```

---

### 3.2.3. Thiết kế lớp

#### Mục đích

Thiết kế cấu trúc mã nguồn hướng đối tượng.

#### Biểu đồ

**Class Diagram**

#### Trả lời câu hỏi

> Chương trình sẽ gồm những class nào?

#### Ví dụ

```text
User
Board
Card
Notification
```

---

### 3.2.4. Thiết kế cơ sở dữ liệu

#### Mục đích

Chuyển mô hình dữ liệu thành các bảng vật lý.

#### Biểu đồ

Có thể sử dụng lại ERD hoặc mô tả bảng dữ liệu.

#### Nội dung

* Tên bảng
* Khóa chính (Primary Key)
* Khóa ngoại (Foreign Key)
* Kiểu dữ liệu

---

### 3.2.5. Thiết kế giao diện

#### Mục đích

Thiết kế các màn hình tương tác với người dùng.

#### Biểu đồ

Không bắt buộc UML.

#### Nội dung

* Mockup
* Wireframe
* Screenshot giao diện

---

# Tóm tắt các biểu đồ

| Mục                       | Biểu đồ           | Trả lời câu hỏi          |
| ------------------------- | ----------------- | ------------------------ |
| Xác định chức năng        | Use Case Diagram  | Hệ thống làm gì?         |
| Phân tích luồng nghiệp vụ | Activity Diagram  | Chức năng chạy thế nào?  |
| Phân tích dữ liệu         | ERD               | Lưu dữ liệu gì?          |
| Thiết kế kiến trúc        | Component Diagram | Chia module thế nào?     |
| Thiết kế tương tác        | Sequence Diagram  | Ai gọi ai?               |
| Thiết kế đối tượng        | Class Diagram     | Có những class nào?      |
| Thiết kế giao diện        | Wireframe/UI      | Người dùng nhìn thấy gì? |

---

# Bộ biểu đồ tối thiểu cho đồ án phần mềm

## Phân tích hệ thống

* Use Case Diagram
* Activity Diagram

## Thiết kế hệ thống

* ERD
* Component Diagram
* Sequence Diagram
* Class Diagram
* Thiết kế giao diện

Đây là bộ tài liệu phổ biến trong các đồ án và khóa luận CNTT, giúp thể hiện đầy đủ quá trình từ **Requirements Analysis → System Analysis → System Design**.


## Chương 4. Triển khai và kết quả

Nếu dự án đã có phần triển khai thì trình bày:

### Các chức năng đã hoàn thành

### Kết quả đạt được

### Hình ảnh minh họa

Khi có hình ảnh:

- Sinh môi trường figure của LaTeX
- Có caption
- Có label

### Đánh giá kết quả

Nêu ưu điểm và hạn chế hiện tại.

---

## Chương 5. Kết luận và hướng phát triển

Bao gồm:

### Kết luận

Tóm tắt:

- Các công việc đã thực hiện
- Các mục tiêu đã đạt được
- Các kết quả đạt được

### Các vấn đề còn tồn tại

Liệt kê:

- Chức năng chưa hoàn thiện
- Hạn chế kỹ thuật
- Các khó khăn còn tồn tại

### Phương hướng phát triển

Nêu các hướng mở rộng trong tương lai.

---

## Quy tắc nhất quán

Tên chức năng, tên module, tên công nghệ và tên cơ sở dữ liệu phải thống nhất trong toàn bộ báo cáo.

Không được mô tả một chức năng ở Chương 4 nếu chức năng đó không xuất hiện trong Chương 3.

---

## Khi được yêu cầu viết một chương

Các chương cần bắt đầu ở một page mới (\clearpage khi kết thúc 1 chương)

Chỉ sinh nội dung của chương được yêu cầu.

Không sinh lại các chương khác.

Không sinh trang bìa hoặc mục lục.

Chỉ trả về mã LaTeX của phần nội dung tương ứng.
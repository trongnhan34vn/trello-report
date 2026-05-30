# Thiết kế cơ sở dữ liệu

## Tổng quan

- **Hệ quản trị:** PostgreSQL
- **Migration tool:** Flyway (`V1__init.sql`)
- **Số bảng chính:** 13 bảng
- **Kiểu khóa chính:** UUID (các bảng nghiệp vụ), INT (các bảng tham chiếu)

---

## Sơ đồ quan hệ tổng thể

```
workspace_categories ──< workspaces ──< workspace_members >── users
                              │                                    │
                              └──< boards ──< board_members >──────┤
                                      │                            │
                                      └──< lists                   │
                                              │                    │
                                              └──< cards ──< card_members >── users
                                                      │
                                                      └──< checklists
                                                               │
                                                               └──< checklist_items

Bảng tham chiếu:
  roles ──< workspace_members
  roles ──< board_members
  images (dùng cho board background_url)
```

---

## Chi tiết từng bảng

### roles
| Cột | Kiểu | Ghi chú |
|---|---|---|
| id | INT PRIMARY KEY | 1=ADMIN, 2=MEMBER, 3=VIEWER |
| name | role_name_enum UNIQUE NOT NULL | Enum: ADMIN, MEMBER, VIEWER |

**Dữ liệu mặc định:**
```sql
INSERT INTO roles VALUES (1, 'ADMIN'), (2, 'MEMBER'), (3, 'VIEWER');
```

---

### images
| Cột | Kiểu | Ghi chú |
|---|---|---|
| id | INT PRIMARY KEY | |
| key | TEXT NOT NULL | Đường dẫn ảnh nền board |

**Dữ liệu mặc định:** 8 ảnh nền (`/images/board_background/background_1.jpg` đến `background_8.jpg`)

---

### users
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| cognito_id | VARCHAR(255) | NOT NULL, UNIQUE |
| full_name | VARCHAR(255) | NOT NULL |
| email | VARCHAR(255) | NOT NULL, UNIQUE |
| avatar_url | VARCHAR(255) | nullable |
| bio | TEXT | nullable |
| phone | VARCHAR(255) | nullable |
| address | VARCHAR(255) | nullable |
| deleted_at | TIMESTAMP | Soft delete |
| created_at | TIMESTAMP | DEFAULT NOW() |
| updated_at | TIMESTAMP | DEFAULT NOW() |

**Liên kết với AWS Cognito:** `cognito_id` = sub trong JWT token của Cognito

---

### workspace_categories
| Cột | Kiểu | Ghi chú |
|---|---|---|
| id | INT PRIMARY KEY | |
| name | VARCHAR(255) NOT NULL | |

**8 loại:** Sales CRM, Education, Human Resources, Operation, Marketing, IT, Small Business, Other

---

### workspaces
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| name | VARCHAR(255) | NOT NULL |
| description | VARCHAR(255) | nullable |
| category_id | INT | FK → workspace_categories(id) |
| created_by | UUID | FK → users(id) ON DELETE SET NULL |
| updated_by | UUID | FK → users(id) ON DELETE SET NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |
| updated_at | TIMESTAMP | DEFAULT NOW() |

---

### boards
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| name | VARCHAR(255) | NOT NULL |
| background_url | TEXT | nullable (chọn từ bảng images) |
| workspace_id | UUID | FK → workspaces(id) **ON DELETE CASCADE** |
| created_by | UUID | FK → users(id) ON DELETE SET NULL |
| updated_by | UUID | FK → users(id) ON DELETE SET NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |
| updated_at | TIMESTAMP | DEFAULT NOW() |

---

### lists
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| name | VARCHAR(255) | NOT NULL |
| board_id | UUID | FK → boards(id) **ON DELETE CASCADE** |
| position | TEXT | NOT NULL — string-based cho drag-and-drop |
| created_by | UUID | FK → users(id) ON DELETE SET NULL |
| updated_by | UUID | FK → users(id) ON DELETE SET NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |
| updated_at | TIMESTAMP | DEFAULT NOW() |

---

### cards
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| title | VARCHAR(255) | NOT NULL |
| description | TEXT | nullable |
| start_date | TIMESTAMP | nullable |
| due_date | TIMESTAMP | nullable |
| is_completed | BOOLEAN | DEFAULT FALSE |
| position | TEXT | NOT NULL — string-based cho drag-and-drop |
| list_id | UUID | FK → lists(id) **ON DELETE CASCADE** |
| created_by | UUID | FK → users(id) ON DELETE SET NULL |
| updated_by | UUID | FK → users(id) ON DELETE SET NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |
| updated_at | TIMESTAMP | DEFAULT NOW() |

---

### checklists
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| name | VARCHAR(255) | NOT NULL |
| card_id | UUID | FK → cards(id) **ON DELETE CASCADE** |
| position | TEXT | NOT NULL |
| created_by | UUID | FK → users(id) ON DELETE SET NULL |
| updated_by | UUID | FK → users(id) ON DELETE SET NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |
| updated_at | TIMESTAMP | DEFAULT NOW() |

---

### checklist_items
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| name | VARCHAR(255) | NOT NULL |
| checklist_id | UUID | FK → checklists(id) **ON DELETE CASCADE** |
| position | TEXT | NOT NULL |
| is_completed | BOOLEAN | NOT NULL DEFAULT FALSE |
| due_date | TIMESTAMP | nullable |
| created_by | UUID | FK → users(id) ON DELETE SET NULL |
| updated_by | UUID | FK → users(id) ON DELETE SET NULL |
| created_at | TIMESTAMP | DEFAULT NOW() |
| updated_at | TIMESTAMP | DEFAULT NOW() |

**Constraint:** `CHECK (is_completed IN (true, false))`

---

### workspace_members
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| user_id | UUID | FK → users(id) ON DELETE CASCADE |
| workspace_id | UUID | FK → workspaces(id) ON DELETE CASCADE |
| role_id | INT | FK → roles(id) ON DELETE CASCADE |
| created_at | TIMESTAMP | DEFAULT NOW() |
| created_by | UUID | FK → users(id) ON DELETE SET NULL |

**Unique:** `(user_id, workspace_id)` — mỗi user chỉ có một role trong workspace

---

### board_members
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| user_id | UUID | FK → users(id) ON DELETE CASCADE |
| board_id | UUID | FK → boards(id) ON DELETE CASCADE |
| role_id | INT | FK → roles(id) ON DELETE CASCADE |
| created_at | TIMESTAMP | DEFAULT NOW() |
| created_by | UUID | FK → users(id) ON DELETE SET NULL |

**Unique:** `(user_id, board_id)` — mỗi user chỉ có một role trong board

---

### card_members
| Cột | Kiểu | Ràng buộc |
|---|---|---|
| id | UUID PRIMARY KEY | |
| user_id | UUID | FK → users(id) ON DELETE CASCADE |
| card_id | UUID | FK → cards(id) ON DELETE CASCADE |
| created_at | TIMESTAMP | DEFAULT NOW() |
| created_by | UUID | FK → users(id) ON DELETE SET NULL |

**Unique:** `(user_id, card_id)` — mỗi user chỉ được gán một lần cho mỗi card

---

## Cascade Delete Chain

```
workspaces
  └── boards          (ON DELETE CASCADE)
        └── lists     (ON DELETE CASCADE)
              └── cards (ON DELETE CASCADE)
                    └── checklists       (ON DELETE CASCADE)
                          └── checklist_items (ON DELETE CASCADE)
                    └── card_members     (ON DELETE CASCADE)
              └── board_members          (ON DELETE CASCADE)
        └── workspace_members            (ON DELETE CASCADE)
```

---

## Thiết kế position (drag-and-drop)

- Kiểu `TEXT` (không phải `INT`) để tránh reindex toàn bộ khi chèn vào giữa
- Thuật toán: dùng giá trị chuỗi trung bình giữa hai phần tử (fractional indexing concept)
- Áp dụng cho: `lists.position`, `cards.position`, `checklists.position`, `checklist_items.position`
- **Lưu ý:** Thuật toán beforeId/afterId còn có bug khi kéo vào đầu/cuối danh sách (W8)

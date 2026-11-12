

## 1\. CẤU TRÚC HỆ THỐNG NHÁNH
bank-ai-affordability-assistant/
├── backend/                          # Spring Boot 3 (Modular Monolith)
│   └── src/main/java/com/bank/assistant/
│       ├── dossier/                  # Tiếp nhận file, quản lý hồ sơ, audit
│       ├── statement/                # Sổ cái giao dịch 6 tháng, gọi FastAPI
│       ├── financial/                # Tính toán tất định (DTI, Residual, Stress)
│       ├── workflow/                 # Camunda 8 Zeebe Workers & BPMN
│       └── shared/                   # Config, Exception, DTO dùng chung
├── ai-service/                       # Python 3.10+ (FastAPI + pdfplumber)
├── frontend/                         # React + Vite + Ant Design
└── docker-compose.yml                # Camunda 8 Self-Managed + PostgreSQL 16
### Nguyên tắc

1. **Không push code trực tiếp lên main hoặc develop.**
2. **Toàn bộ code mới bắt buộc phải đi qua nhánh `feature/\*` và gộp vào `develop` thông qua Pull Request.**
3. **Nhánh main chỉ nhận code từ develop vào ngày cuối cùng của Sprint khi toàn bộ kịch bản kiểm thử đã pass.**

\---

## 2\. QUY ƯỚC ĐẶT TÊN NHÁNH

Tất cả các nhánh con đều phải được tạo từ nhánh `develop` mới nhất và tuân thủ định dạng:

feature/<Jira-Key>-<mo-ta-ngan-gon>

<Jira-Key> sẽ được tự tạo khi PO tạo task trên jira và phải commit theo quy tắc trên để jira chuyển sang done khi pr.

\---

## 3\. QUY TRÌNH LÀM VIỆC TỪNG BƯỚC

### Bước 1: Đồng bộ mã nguồn đầu ngày \& Tạo nhánh mới

Trước khi bắt đầu làm một feature mới:


# Chuyển về nhánh develop và kéo code mới nhất của cả nhóm về
git checkout develop
git pull origin develop

# Tạo nhánh tính năng mới từ develop
git checkout -b feature/be-calculate-dti


\---

### Bước 2: Viết code và Commit trên máy cá nhân

Chỉ làm việc trong thư mục phụ trách.

* **Quy tắc Commit Message:**  
Cú pháp: `<type>(<scope>): <mô tả ngắn bằng tiếng Anh>`

\---

### Bước 3: Đồng bộ trước khi tạo Pull Request

Sau khi code xong và test chạy thử trên local đã pass, trước khi tạo pr, phải kéo cập nhật mới nhất từ nhánh develop vào nhánh của mình để xử lý xung đột nếu có.
Nếu có conflict -> giải quyết conflict -> commit -> test lại
---

### Bước 4: Tạo Pull Request & Code Review

1. Vào GitHub repo của dự án, bấm **New Pull Request**.
2. **Thiết lập:**

   * `base`: **`develop`**.
   * `compare`: **`feature/be-calculate-dti`**.
3. **Tiêu đề PR:** `\[BE] <mô tả tính năng đã thêm bằng tiếng việt>`
4. **Description 2 mục:**
* **Phạm vi thay đổi:** Đã thêm/sửa những file nào trong thư mục?
* **Cách kiểm thử:** Hướng dẫn các tham số request JSON để test trên Postman.
5. **Gán Reviewer:**
* Gán cho PO hoặc các bạn cùng team.

\---

### Bước 5: Review \& Merge vào nhánh `develop`

* **Người Review: sau khi đã kiểm tra xong nhấn **Approve**.
* **Người tạo PR:**

  * Chọn phương thức **Squash and merge** hoặc **Merge commit** vào `develop`.
  * Bấm **Delete branch** trên GitHub để dọn dẹp các nhánh đã hoàn tất.

\---

## 4\. QUY TRÌNH ĐÓNG SPRINT \& MERGE VÀO NHÁNH MAIN

Vào ngày cuối cùng của Sprint:

1. **Kiểm thử tích hợp cuối cùng:** Tester lấy nhánh `develop` mới nhất, chạy kiểm thử toàn bộ luồng E2E.
2. **Khóa nhánh:** Không ai được merge thêm PR mới vào `develop`.
3. **Tạo Release PR:**

   * Scrum Master tạo PR từ **`develop` vào `main`**.
   * Tiêu đề: `Release: Sprint 1 Completion`
4. **Gắn Tag phiên bản trên nhánh `main`:**
   git checkout main
   git pull origin main
   git tag -a v1-sprint1 -m "Ban hoan thien nghiem thu Sprint 1"
   git push origin v1.0.0-sprint1

QUY CHUẨN DATABASE MIGRATION -> không sửa bảng trực tiếp bằng công cụ GUI -> quản lí bảng bằng flyway
Quy tắc đặt tên V1__<mo-ta-ngan-gon-bang-tieng-anh>.sql ví dụ: V1__init_dossier_schema.sql
\---


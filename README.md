
## 1. Yêu cầu môi trường
* **Java:** OpenJDK 21 (LTS)
* **Build Tool:** Maven 3.9+ (hoặc dùng sẵn `./mvnw` / `mvnw.cmd`)
* **Database:** PostgreSQL 15+
* **Workflow Engine:** Camunda 8 (Zeebe Gateway `localhost:26500`)
* **Python AI Service:** FastApi OCR/Extraction Service (`localhost:8000`)

---

## 2. Cấu trúc thư mục Modular Monolith

```text
src/main/java/vn/unit/affordability/
├── AffordabilityAssistantApplication.java  # Main Application Entrypoint
├── dossier/                                # Module quản lý hồ sơ thẩm định & phân quyền Maker/Checker
├── statement/                              # Module tiếp nhận sao kê, bóc tách và đối soát lương 6 tháng
├── financial/                              # Module tính toán tài chính số học (DTI, chi phí sàn, 3 kịch bản stress)
├── workflow/                               # Module tích hợp Camunda 8 BPMN/DMN, Zeebe Job Workers
└── shared/                                 # Cấu hình dùng chung, WebClient, Exception handling, Outbox event
```

---

## 3. Cấu hình bảo mật & Chạy ứng dụng

### Bước 1: Tạo cấu hình cục bộ từ file mẫu

```bash
# Windows PowerShell
Copy-Item src/main/resources/application.example.yml src/main/resources/application-local.yml
```

### Bước 2: Cập nhật thông tin trong `application-local.yml`
* Điền mật khẩu database PostgreSQL (`password: your_password`).
* Kiểm tra port của PostgreSQL, Zeebe Gateway (26500) và Python AI Service (8000).

### Bước 3: Build và khởi chạy
```bash
# Chạy kiểm thử
mvn clean test

# Chạy ứng dụng Spring Boot
mvn spring-boot:run
```
Server sẽ khởi chạy tại: `http://localhost:8080`.


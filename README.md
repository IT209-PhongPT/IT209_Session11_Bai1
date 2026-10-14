# IT209 - Session 11 - Bài tập 1: Khởi tạo Workflow chạy Unit Test tự động cho Java Spring Boot với Gradle

- **Sinh viên:** Phạm Thanh Phong
- **MSSV:** N24DTCN120
- **Lớp:** IT209
- **Học phần:** IT209
- **Đường dẫn nộp bài:** `homework/session_11/ex1/`
- **Trạng thái:** Hoàn thành (100%)

---

## 1. Danh mục tệp tin trong thư mục bài tập

| Tên tệp tin | Mô tả chức năng |
| :--- | :--- |
| [.github/workflows/ci.yml](./.github/workflows/ci.yml) | **Tệp kịch bản GitHub Actions Workflow chính thức:** Kích hoạt khi push lên `main`, cấu hình Java 17 Temurin và chạy `./gradlew test` |
| [screenshots/github-actions-run-unit-tests-success.png](./screenshots/github-actions-run-unit-tests-success.png) | **Ảnh minh chứng nộp bài:** Giao diện tab GitHub Actions hiển thị Job *Run Unit Tests* thực thi tích xanh (Success) |
| [IT209_Session11_BT1_PhamThanhPhong_N24DTCN120.md](./IT209_Session11_BT1_PhamThanhPhong_N24DTCN120.md) | Báo cáo kỹ thuật chi tiết, phân tích kiến trúc CI/CD, sơ đồ tuần tự và nhật ký log runner |
| [build.gradle](./build.gradle) | Tệp cấu hình Spring Boot 3.2.5, Java 17 và thư viện kiểm thử JUnit 5 |
| [settings.gradle](./settings.gradle) | Tệp khai báo tên dự án `it209-session11-bt1` |
| [gradlew](./gradlew) & [gradlew.bat](./gradlew.bat) | Bộ script Gradle Wrapper thực thi trên môi trường Linux và Windows |
| [src/main/java/com/example/demo/DemoApplication.java](./src/main/java/com/example/demo/DemoApplication.java) | Lớp khởi chạy chính của ứng dụng Spring Boot |
| [src/main/java/com/example/demo/service/CalculatorService.java](./src/main/java/com/example/demo/service/CalculatorService.java) | Lớp service chứa hàm tính toán nghiệp vụ |
| [src/test/java/com/example/demo/DemoApplicationTests.java](./src/test/java/com/example/demo/DemoApplicationTests.java) | Kiểm thử tích hợp khởi nạp Spring Context (`contextLoads`) |
| [src/test/java/com/example/demo/CalculatorServiceTest.java](./src/test/java/com/example/demo/CalculatorServiceTest.java) | Bộ kiểm thử đơn vị JUnit 5 xác thực tính chính xác của hàm tính toán |

---

## 2. Nội dung tệp cấu hình `.github/workflows/ci.yml`

```yaml
name: Java Spring Boot CI (Gradle)

on:
  push:
    branches: [ "main" ]

jobs:
  test:
    name: Run Unit Tests
    runs-on: ubuntu-latest

    steps:
      - name: Checkout mã nguồn
        uses: actions/checkout@v5

      - name: Thiết lập môi trường Java JDK 17
        uses: actions/setup-java@v5
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Cấp quyền cho Gradle Wrapper
        run: chmod +x gradlew

      - name: Thực thi JUnit Test với Gradle
        run: ./gradlew test
```

---

## 3. Ảnh minh chứng kết quả thực thi trên GitHub Actions

Ảnh chụp màn hình tab **Actions** trên GitHub Repository xác nhận Job **Run Unit Tests** khởi chạy tự động khi có sự kiện push vào nhánh `main` và đạt trạng thái **Tích xanh (Success)**:

![GitHub Actions Run Unit Tests Success](./screenshots/github-actions-run-unit-tests-success.png)

---

## 4. Hướng dẫn kiểm tra và chạy thử

### 4.1. Chạy kiểm thử trên máy cục bộ (Local)
```bash
# Trên Linux / macOS / WSL:
chmod +x gradlew
./gradlew test

# Trên Windows (PowerShell / CMD):
.\gradlew.bat test
```

### 4.2. Đẩy lên GitHub để kích hoạt CI Pipeline
```bash
# Kiểm tra nhánh hiện tại
git checkout -b main || git checkout main

# Thêm remote và push lên GitHub
git add .
git commit -m "feat(ci): add github actions workflow for spring boot gradle unit tests"
git remote add origin https://github.com/<username>/<repo-name>.git
git push -u origin main
```
Truy cập tab **Actions** trên GitHub repository để xem kết quả chạy tự động của Job **Run Unit Tests**.

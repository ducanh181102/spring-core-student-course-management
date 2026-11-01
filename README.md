# Spring Core Student Course Management

Console app quản lý học viên, khóa học và đăng ký khóa học. Dự án dùng Spring Core để quản lý dependency, Spring JDBC để truy cập cơ sở dữ liệu H2 và Spring Transaction để bảo đảm các thao tác xóa liên quan được thực hiện nhất quán.

## Chức năng

### Học viên

- Xem danh sách học viên.
- Thêm, sửa tên và xóa học viên.
- Khi xóa học viên, các đăng ký khóa học của học viên đó cũng được xóa trong cùng transaction.

### Khóa học

- Xem danh sách khóa học.
- Thêm khóa học, sửa tên hoặc sức chứa.
- Không cho phép sức chứa nhỏ hơn số học viên đã đăng ký.
- Khi xóa khóa học, các đăng ký của khóa học đó cũng được xóa trong cùng transaction.

### Đăng ký

- Xem danh sách đăng ký.
- Đăng ký học viên vào khóa học hoặc hủy đăng ký.
- Kiểm tra học viên và khóa học tồn tại, không đăng ký trùng và khóa học chưa đủ sức chứa.

## Công nghệ

- Java 21
- Spring Context (Spring Core / IoC / Dependency Injection)
- Spring JDBC (`JdbcTemplate`)
- Spring Transaction (`@Transactional`, `DataSourceTransactionManager`)
- H2 Database ở chế độ lưu trữ trên file
- JUnit Jupiter
- Maven

## Yêu cầu

- JDK 21
- Maven 3.9 trở lên

Kiểm tra cài đặt:

```bash
java -version
mvn -version
```

## Chạy ứng dụng

Mở thư mục dự án trong IntelliJ IDEA, tải lại cấu hình Maven, sau đó chạy `src/main/java/org/example/school/Main.java`.

Có thể chạy từ terminal trên macOS/Linux:

```bash
mvn compile dependency:build-classpath -Dmdep.outputFile=target/classpath.txt
java -cp "target/classes:$(cat target/classpath.txt)" org.example.school.Main
```

Khi chạy, chọn chức năng trong menu console bằng cách nhập số từ `0` đến `12`.

## Chạy kiểm thử

```bash
mvn test
```

Các bài kiểm thử nằm trong `src/test/java`. Maven báo kết quả tổng hợp trong `target/surefire-reports`.

## Cơ sở dữ liệu

- Cấu hình kết nối nằm trong `AppConfig`.
- H2 dùng URL `jdbc:h2:file:./data/school` và tạo cơ sở dữ liệu trong thư mục `data` tính từ working directory.
- Lược đồ bảng được khởi tạo từ `src/main/resources/schema.sql`.
- Các bảng gồm `student`, `course` và `enrollment`; bảng đăng ký có khóa ngoại đến học viên và khóa học.
- Thư mục `data` được Git bỏ qua, vì vậy dữ liệu cục bộ không được đưa lên repository. Chạy ứng dụng lần đầu sẽ tạo database và các bảng nếu chúng chưa tồn tại.

## Cấu trúc chính

```text
src/
├── main/
│   ├── java/org/example/school/
│   │   ├── AppConfig.java
│   │   ├── Main.java
│   │   ├── Student, Course, Enrollment.java
│   │   ├── *Repository.java
│   │   └── *Service.java
│   └── resources/schema.sql
└── test/java/org/example/school/
```

- `Main`: menu console và nhận dữ liệu đầu vào.
- `AppConfig`: cấu hình Spring, `DataSource`, `JdbcTemplate`, transaction manager và khởi tạo schema.
- `Repository`: truy vấn và cập nhật dữ liệu bằng `JdbcTemplate`.
- `Service`: xử lý nghiệp vụ và kiểm tra dữ liệu trước khi gọi repository.

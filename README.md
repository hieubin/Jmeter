# BÁO CÁO TÌM HIỂU VÀ THỰC HÀNH KIỂM THỬ HIỆU NĂNG VỚI APACHE JMETER

- **Sinh viên:** Họ và tên – MSSV
- **Lớp:** ………
- **Môn học:** Kiểm thử phần mềm
- **Giảng viên:** ………

---

## 1. Giới thiệu về JMeter

**Apache JMeter** là công cụ mã nguồn mở viết bằng Java, dùng để **kiểm thử hiệu năng (performance testing)** và kiểm thử chức năng cho ứng dụng web, API, cơ sở dữ liệu, FTP, ...

Các khái niệm cơ bản:

| Thành phần | Vai trò |
|---|---|
| **Test Plan** | Kịch bản kiểm thử tổng thể |
| **Thread Group** | Nhóm người dùng ảo (số user, ramp-up, số vòng lặp) |
| **Sampler** | Request gửi đi (HTTP Request, JDBC, FTP, ...) |
| **Config Element** | Cấu hình dùng chung (HTTP Request Defaults, Header Manager, ...) |
| **Assertion** | Kiểm tra kết quả trả về (mã 200, nội dung chứa chuỗi, ...) |
| **Listener** | Hiển thị / lưu kết quả (View Results Tree, Summary Report, ...) |

Ba tham số quan trọng của Thread Group:
- **Number of Threads (users):** số người dùng ảo chạy đồng thời.
- **Ramp-up period:** thời gian (giây) để khởi động hết các user.
- **Loop Count:** số lần mỗi user lặp lại kịch bản.

**Tài liệu tham khảo**
- Trang chủ: https://jmeter.apache.org
- Hướng dẫn sử dụng: https://jmeter.apache.org/usermanual/index.html
- Video hướng dẫn trên YouTube (tìm: "JMeter tutorial for beginners")

---

## 2. Cài đặt môi trường

1. Cài **Java JDK 8 trở lên** (kiểm tra bằng `java -version`).
2. Tải JMeter tại https://jmeter.apache.org/download_jmeter.cgi (file `.zip`), giải nén.
3. Chạy `bin/jmeter.bat` (Windows) hoặc `bin/jmeter.sh` (Linux/macOS).

![Giao diện JMeter sau khi mở](images/01-jmeter-home.png)

---

## 3. Đối tượng kiểm thử

Sử dụng **JSONPlaceholder** (https://jsonplaceholder.typicode.com) – dịch vụ REST API giả lập miễn phí, được tạo ra để phục vụ học tập và kiểm thử.

| STT | API | Phương thức | Mã mong đợi |
|---|---|---|---|
| 1 | `/posts` | GET | 200 |
| 2 | `/posts/1` | GET | 200 |
| 3 | `/posts` (kèm body JSON) | POST | 201 |

> Lưu ý: chỉ kiểm thử tải trên các trang cho phép (như trang demo/API giả lập). Không chạy tải lớn lên website thật của người khác.

---

## 4. Xây dựng kịch bản kiểm thử

### 4.1. Tạo Thread Group

- Number of Threads: **20**
- Ramp-up period: **10 giây**
- Loop Count: **5**
- Tổng số request mỗi API: 20 × 5 = 100

![Cấu hình Thread Group](images/02-thread-group.png)

### 4.2. HTTP Request Defaults

- Protocol: `https`
- Server Name: `jsonplaceholder.typicode.com`

![HTTP Request Defaults](images/03-http-defaults.png)

### 4.3. Các HTTP Request

![Danh sách HTTP Request](images/04-http-requests.png)

Request POST có thêm **HTTP Header Manager** với `Content-Type: application/json` và body:

```json
{"title":"Kiem thu JMeter","body":"Bai tap kiem thu hieu nang","userId":1}
```

![Cấu hình request POST](images/05-post-request.png)

### 4.4. Response Assertion

Mỗi request có một Response Assertion kiểm tra **Response Code** bằng 200 (GET) hoặc 201 (POST).

![Response Assertion](images/06-assertion.png)

### 4.5. Listener

Thêm 3 listener: **View Results Tree**, **Summary Report**, **Aggregate Report**.

![Cây Test Plan hoàn chỉnh](images/07-test-plan-tree.png)

---

## 5. Kết quả thực hiện

Nhấn nút **Start** (▶) để chạy kịch bản.

### 5.1. View Results Tree

![View Results Tree](images/08-results-tree.png)

### 5.2. Summary Report

![Summary Report](images/09-summary-report.png)

### 5.3. Aggregate Report

![Aggregate Report](images/10-aggregate-report.png)

### 5.4. Bảng tổng hợp số liệu

> Điền số liệu theo ảnh chụp của bạn.

| Request | Samples | Average (ms) | Min (ms) | Max (ms) | Error % | Throughput (req/s) |
|---|---|---|---|---|---|---|
| GET /posts | | | | | | |
| GET /posts/1 | | | | | | |
| POST /posts | | | | | | |
| **TOTAL** | | | | | | |

---

## 6. Nhận xét và đánh giá

> Viết bằng lời của bạn, gợi ý:

- Tỉ lệ lỗi (Error %) là bao nhiêu? Có request nào thất bại không?
- Thời gian phản hồi trung bình (Average) và lớn nhất (Max) có chấp nhận được không?
- API nào chậm nhất? Vì sao có thể như vậy (dữ liệu trả về lớn, POST xử lý lâu hơn, ...)?
- Khi tăng số user (ví dụ 20 → 50 → 100) thì Throughput và Average thay đổi thế nào?

---

## 7. Kết luận

Tóm tắt những gì đã học được: cách tạo Test Plan, cấu hình Thread Group, gửi request GET/POST, dùng Assertion và đọc các báo cáo của JMeter. Nêu khó khăn gặp phải và hướng phát triển (ví dụ: thử với dữ liệu CSV, dùng Timer, chạy chế độ non-GUI).

---

## 8. Cấu trúc repo

```
.
├── README.md          # Báo cáo
├── test-plan.jmx      # Kịch bản JMeter (mở bằng File > Open)
└── images/            # Ảnh minh hoạ
```

## 9. Cách chạy lại kịch bản

**Bằng giao diện:** mở JMeter → `File > Open` → chọn `test-plan.jmx` → nhấn **Start**.

**Bằng dòng lệnh (non-GUI):**

```bash
jmeter -n -t test-plan.jmx -l result.jtl -e -o report/
```

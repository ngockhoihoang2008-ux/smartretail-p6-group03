# SmartRetail - Đề P6: Store Intelligence & Alert Engine (Nhóm 03)

## 1. Thành viên & Giảng viên
- **Giảng viên hướng dẫn:** `vvvanthe`
- **Thành viên nhóm:**
  1. Hoàng Ngọc Khôi (`ngockhoihoang2008-ux`) - Team Leader
  2. Bảo (`lengqbao10708-code`)
  3. Long (`adamnguyen443-byte`)
  4. Hiền (`NguyenThuHien`)

## 2. Đặc tả bài toán
- **Input:** Đọc 1 tệp CSV `daily_store.csv` trong thư mục dữ liệu (`data/sample`, `data/full`, `data/edge/empty`, `data/edge/dirty`) gồm các cột: `date, store_id, visitors, orders, revenue, target_revenue, stockout_products`. Tham số dòng lệnh: `p6 <thư_mục_dữ_liệu> [store_id] [date]` (mặc định `store_id = S001`, `date` = ngày mới nhất có dữ liệu của cửa hàng).
- **Xử lý:** 
  - Đọc dữ liệu vào `vector` của `struct`, bỏ qua các dòng lỗi (giá trị âm, `target_revenue = 0`, sai ngày, thiếu cột) và in cảnh báo kèm số dòng ra `cerr`.
  - Tính KPI (`Conversion = orders / visitors * 100%`, `Target achievement = revenue / target_revenue * 100%`) và so sánh mức tăng/giảm với ngày có dữ liệu liền trước.
  - Xét 3 luật cảnh báo theo thứ tự (`stockout_products > 3` [HIGH], `conversion < 20%` [HIGH], `target achievement < 80%` [MEDIUM]), sắp xếp ổn định HIGH trước MEDIUM bằng bubble sort và sinh hành động đề xuất trạng thái `PENDING`.
- **Output:** In báo cáo KPI ngày được chọn (`DAILY STORE REPORT`), danh sách cảnh báo (`ALERTS`), hành động chờ duyệt (`RECOMMENDED ACTIONS`), bảng xu hướng mọi ngày (`TREND`) và tổng kết (`SUMMARY`).

## 3. Cách build và chạy
```bash
mkdir -p bin
g++ -std=c++17 -Wall -Wextra src/main.cpp -o bin/p6
./bin/p6 data/sample S001 2026-08-21
```

## 4. Giả định và giới hạn
- Ngày được lưu và so sánh theo định dạng chuỗi `YYYY-MM-DD`. Doanh thu (`revenue`, `target_revenue`) lưu bằng kiểu `long long` (VND).
- Nếu `visitors == 0` (cửa hàng đóng cửa), tỷ lệ chuyển đổi in `n/a`, không thực hiện phép chia cho 0, không sinh cảnh báo `Low Conversion` và không tính vào ngày có conversion thấp nhất.
- Chương trình chỉ sử dụng kiến thức cơ bản C++ (hàm, `struct`, `vector`, chuỗi, đọc tệp, tìm kiếm nhị phân đệ quy, bubble sort tự viết; không dùng class, con trỏ hay `std::sort`).

## 5. Project KHÔNG làm gì (Ranh giới)
- Không tự động đánh dấu một hành động đề xuất là `APPROVED`.
- Không tính toán số lượng hàng hóa cần nhập thêm.
- Không đọc các tệp CSV khác của hệ thống ngoài `daily_store.csv` và không chỉnh sửa dữ liệu gốc trong `data/`.

## 6. Task assignment table
| ID | Task | Function/file | Assignee | Reviewer | Flowchart | PR |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| T01 | Repo skeleton, PR template & README | `.gitignore`, `pull_request_template.md`, `README.md` | Khôi | Bảo | not needed | #1, #2 |
| T02 | Nạp bộ dữ liệu CSV chuẩn vào thư mục dự án | `data/sample/`, `data/full/`, `data/edge/` | Hiền | Khôi | not needed | |
| T03 | Khai báo cấu trúc dữ liệu (`struct`) & hàm khởi tạo | `struct DayRecord`, `struct Alert` trong `src/main.cpp` | Hiền | Long | yes | |
| T04 | Đọc `daily_store.csv`, lọc dòng lỗi & cảnh báo `cerr` | `loadDailyStore()` | Khôi | Bảo | yes | |
| T05 | Lọc theo `store_id` & tìm kiếm nhị phân đệ quy | `filterByStore()`, `findDay()` | Khôi | Long | yes | |
| T06 | **Hàm lõi:** Kiểm tra 3 luật cảnh báo, đánh giá ngày & Bubble Sort ổn định | `checkInventory()`, `checkConversion()`, `checkTarget()`, `evaluateDay()`, `sortAlerts()` | Bảo | Khôi | yes | |
| T07 | Tìm ngày liền trước & tính KPI (`Conversion`, `Target`) | `previousDay()`, `computeKPI()` | Long | Khôi | yes | |
| T08 | In báo cáo ngày (`DAILY STORE REPORT`, `ALERTS`, `ACTIONS`) | `printReport()` | Long | Bảo | yes | |
| T09 | In bảng xu hướng mọi ngày (`TREND`) & thống kê (`SUMMARY`) | `printTrend()`, `printSummary()` | Bảo | Hiền | yes | |
| T10 | Ghép luồng chính `main()` & kiểm tra tham số dòng lệnh | `main()` trong `src/main.cpp` | Khôi | Bảo | yes | |
| T11 | Bộ kiểm thử bắt buộc `TC01–TC07` & dữ liệu `expected/` | `tests/test_cases.md`, `tests/expected/` | Long | Hiền | not needed | |
| T12 | Tài liệu kiến trúc tổng thể, sơ đồ hàm & thuật toán | `docs/architecture.md` | Bảo | Khôi | not needed | |
| T13 | Thiết kế slide thuyết trình nhóm (tối đa 10 trang) | `docs/slides.pdf` | Hiền | Khôi | not needed | |


## 7. Khai báo AI và nguồn tham khảo
- None (nhóm sẽ cập nhật chi tiết công cụ AI và phần code gợi ý nếu có sử dụng trong từng PR).

# SmartRetail - Đề P6: Store Intelligence & Alert Engine (Nhóm 03)

## 1. Thành viên & Giảng viên
- **Giảng viên hướng dẫn:** `vvvanthe`
- **Thành viên nhóm:**
  1. Hoàng Ngọc Khôi (`ngockhoihoang2008-ux`) - Team Leader
  2. [Họ tên thành viên 2] (`adamnguyen443-byte`)
  3. [Họ tên thành viên 3] (`lngqb`)
  4. [Họ tên thành viên 4] (`NguyenThuHien`)

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
- Chương trình chỉ sử dụng kiến thức cơ bản C++ (hàm, `struct`, `vector`, chuỗi, đọc tệp, tìm kiếm đệ quy, bubble sort tự viết; không dùng class, con trỏ hay `std::sort`).

## 5. Project KHÔNG làm gì (Ranh giới)
- Không tự động đánh dấu một hành động đề xuất là `APPROVED`.
- Không tính toán số lượng hàng hóa cần nhập thêm.
- Không đọc các tệp CSV khác của hệ thống ngoài `daily_store.csv` và không chỉnh sửa dữ liệu gốc trong `data/`.

## 6. Task assignment table
| ID | Task | Function/file | Assignee | Reviewer | Flowchart | PR |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| T01 | Repo skeleton & PR template | `.gitignore`, `pull_request_template.md`, `README.md` | Hoàng Ngọc Khôi | [Tên Reviewer] | not needed | #1 |
| T02 | Đọc dữ liệu `daily_store.csv` & kiểm tra dòng lỗi | `loadDailyStore()` | [Tên thành viên] | [Tên Reviewer] | yes | |
| T03 | Tìm kiếm ngày (đệ quy) & lấy ngày liền trước | `findDay()`, `previousDay()` | [Tên thành viên] | [Tên Reviewer] | yes | |
| T04 | Hàm lõi: Kiểm tra 3 luật cảnh báo & sắp xếp (bubble sort) | `checkInventory()`, `checkConversion()`, `checkTarget()`, `evaluateDay()` | [Tên thành viên] | [Tên Reviewer] | yes | |
| T05 | In báo cáo ngày, bảng TREND và phần SUMMARY | `printReport()`, `printTrend()` | [Tên thành viên] | [Tên Reviewer] | yes | |
| T06 | Bộ test cases bắt buộc (TC01 - TC07) | `tests/test_cases.md` | [Tên thành viên] | [Tên Reviewer] | not needed | |
| T07 | Tài liệu kiến trúc & Slide báo cáo | `docs/architecture.md`, `docs/slides.pdf` | [Tên thành viên] | [Tên Reviewer] | not needed | |

## 7. Khai báo AI và nguồn tham khảo
- None (nhóm sẽ cập nhật chi tiết công cụ AI và phần code gợi ý nếu có sử dụng trong từng PR).

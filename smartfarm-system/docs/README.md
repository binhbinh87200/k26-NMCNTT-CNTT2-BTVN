# SmartFarm Monitor - Hệ Thống Giám Sát Trạm Cảm Biến Nông Trại

Hệ thống hỗ trợ kỹ sư hạ tầng SmartFarm lọc nhật ký (log) máy chủ tự động và quản lý danh sách thiết bị của các trạm cảm biến ngoài trang trại.

```

## Hướng dẫn chạy script

Mở PowerShell, di chuyển vào thư mục `scripts/` rồi chạy script lọc log:

```powershell
cd smartfarm-system\scripts
.\auto_report.ps1

```

Nếu bị chặn bởi ExecutionPolicy, chạy bằng lệnh sau:

```powershell
powershell -ExecutionPolicy Bypass -File .\auto_report.ps1

```

Kết quả được ghi vào `backup/bao-cao-su-co.txt`.

## Checklist kiểm tra thiết bị

-   [x] Kiểm tra nguồn điện của các trạm cảm biến
-   [ ] Kiểm tra kết nối mạng và modem tại từng trạm
-   [ ] Kiểm tra dung lượng RAM và bộ nhớ lưu trữ
-   [ ] Đối soát mã linh kiện với danh mục trong Excel

## Bảng thông số phần cứng

eMMC 8GB

Bộ điều khiển trung tâm

2.500.000

SSD 64GB

## Phân tích hệ thống

### Mô hình IPO của chức năng lọc log

Thành phần

Nội dung

**Input** =log-he-thong.log gồm các dòng nhật ký dạng ngày giờ  thiết bị nội dung với các mức INFO, WARNING, ERROR, CRITICAL

**Process** 

`Get-Content` đọc file log, `Select-String` lọc các dòng chứa CRITICAL hoặc ERROR, `Measure-Object` đếm số dòng lỗi, toán tử `>>` nối kết quả vào file báo cáo

**Output** 

File `backup/bao-cao-su-co.txt` gồm tiêu đề kèm ngày giờ, danh sách các dòng lỗi và tổng số sự cố

### Phân loại theo tầng DIKW

(a) "Quyết định thay modem trạm sensor-03 trước thứ sáu."

**Wisdom**

Là quyết định hành động dựa trên hiểu biết, có cân nhắc thời hạn

(b) "2026-10-12 08:15:01 [ERROR] sensor-03 Connection TIMEOUT after 30s"

**Data**

Là dòng log thô, rời rạc, chưa được xử lý hay gắn ý nghĩa

(c) "Hôm nay hệ thống ghi nhận 8 sự cố, trong đó 4 sự cố đến từ trạm sensor-03."

**Information**

Dữ liệu đã được tổng hợp, đếm và có ngữ cảnh (hôm nay, theo trạm)

(d) "Trạm sensor-03 hay mất kết nối vào buổi chiều do sóng yếu, nên cần lắp thêm anten."

**Knowledge**

Là quy luật và nguyên nhân rút ra từ nhiều thông tin, kèm cách giải quyết chung

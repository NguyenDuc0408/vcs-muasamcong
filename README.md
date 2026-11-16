# vcs-muasamcong

Dự án tính toán thông tin mua sắm công, dữ liệu được lấy từ trang public https://muasamcong.mof.gov.vn/

## Mục đích

Từ **bản tin đấu thầu** (file Excel liệt kê các Số TBMT), hệ thống tự tra cứu trên Hệ thống mạng đấu thầu quốc gia (muasamcong.mof.gov.vn) rồi tạo ra **file Excel kết quả** gồm:

- Trạng thái từng TBMT (đang xét thầu, huỷ, không có nhà thầu trúng, hoặc tên nhà thầu trúng).
- Kế hoạch lựa chọn nhà thầu (KHLCNT) tương ứng với từng TBMT.
- Phân loại các gói thầu thuộc lĩnh vực **An toàn thông tin (ATTT)** theo danh sách từ khoá.
- Tổng tiền kế hoạch ATTT và dự toán còn lại theo từng chủ đầu tư, từng năm.
- Thống kê nhà thầu trúng thầu.

## Cấu trúc thư mục

```
vcs-muasamcong/
├── README.md              # Tài liệu tổng quan (file này)
├── origin_files/          # Đầu vào: các file bản tin đấu thầu gốc (.xlsx)
│   └── README.md
└── calculated_files/      # Đầu ra: các file kết quả đã tính toán (.xlsx)
    └── README.md
```

| Thư mục | Vai trò | Ai ghi vào |
|---|---|---|
| [origin_files/](origin_files/README.md) | Chứa file bản tin gốc | Người dùng upload thủ công |
| [calculated_files/](calculated_files/README.md) | Chứa file kết quả theo từng lần chạy | Server tự động commit |

## Luồng xử lý

```
 Người dùng upload file bản tin
            │
            ▼
   origin_files/<tên file>.xlsx
            │   (server chạy cron, git pull)
            ▼
   Chọn file mới nhất (theo thời điểm commit) chưa được xử lý
            │
            ▼
   Chạy script tra_cuu_muasamcong.py
   (gọi API muasamcong: TBMT → kế hoạch → gói thầu → TBMT bổ sung)
            │
            ▼
   calculated_files/<tên file>_<YYYYMMDD_HHMMSS>.xlsx
            │   (server commit + push)
            ▼
          GitHub
```

1. Người dùng upload bản tin mới vào `origin_files/`.
2. Server có cron định kỳ `git pull`, xác định file mới nhất (theo thời điểm commit lên GitHub) và so với file đã xử lý lần trước. Chỉ xử lý khi có file mới.
3. Server chạy script tra cứu, sinh file kết quả.
4. File kết quả được đặt vào `calculated_files/` với **cùng tên file gốc, thêm hậu tố thời gian lúc chạy**, rồi commit và push lên GitHub.

> Trạng thái: thư mục `origin_files/` và `calculated_files/` đã được tạo. Script tra cứu hiện nằm ở thư mục `mua_sam_cong/` (ngoài repo này) và phần tự động hoá bằng cron đang trong giai đoạn thiết kế.

## Kết quả gồm những gì

File kết quả giữ nguyên sheet gốc và thêm các sheet tính toán:

| Sheet | Nội dung |
|---|---|
| `Muasamcong` | Sheet gốc của bản tin, **không bị sửa** |
| `muasamcong_refactor` | Các dòng từ năm 2023 trở đi, đã bỏ TBMT trạng thái `DHT`, chỉ giữ một dòng cho mỗi TBMT (phiên bản mới nhất), kèm các cột do công cụ bổ sung |
| `Gói ATTT` | Danh sách gói thầu thuộc lĩnh vực ATTT, kèm giá và trạng thái đã có nhà thầu trúng hay chưa |
| `Kế hoạch chủ đầu tư` | Tổng hợp theo năm và chủ đầu tư: tổng tiền kế hoạch ATTT, dự toán còn lại, chênh lệch và ghi chú |
| `Nhà thầu trúng thầu` | Tổng hợp theo năm và nhà thầu: số gói và tổng giá trúng |

Các ô tổng hợp là **công thức Excel** tham chiếu về sheet dữ liệu nên có thể kiểm tra lại. Excel sẽ tự tính khi mở file. Các trình xem trước (ví dụ xem trực tiếp trên GitHub hoặc chế độ Protected View) có thể hiển thị ô trống.

Nhãn trong cột "Đơn vị trúng thầu" của sheet `muasamcong_refactor`:

| Nhãn | Ý nghĩa |
|---|---|
| Tên nhà thầu | TBMT đã có nhà thầu trúng |
| Đang xét thầu | Chưa có kết quả lựa chọn nhà thầu |
| Không có nhà thầu trúng | Gói thầu đã kết thúc nhưng không có nhà thầu trúng |
| Huỷ TBMT | TBMT đã bị huỷ, không tính vào tổng tiền kế hoạch của chủ đầu tư |

## Cấu hình và bảo mật

- Công cụ cần một **token** để gọi API, đặt trong `config.yaml`. File này **tuyệt đối không commit** lên repo.
- Danh sách từ khoá nhận diện gói thầu ATTT (`attt_keywords`: strong / weak / exclude) cũng nằm trong `config.yaml`.
- Các file cache JSON (`*_api.json`, `*_plan.json`, `*_goi_thau.json`, `*_tbmt_goi.json`) chỉ phục vụ chạy nhanh hơn, giữ trên server và **không commit**.
- Bản tin đấu thầu và file kết quả chứa thông tin nội bộ (kế hoạch, dự toán). Nên đặt repo ở chế độ **private**.

## Hướng dẫn sử dụng script

Chi tiết cách chạy thủ công, các tham số dòng lệnh và giải thích từng cột xem trong `HUONG_DAN_SU_DUNG.md` của thư mục `mua_sam_cong/`.

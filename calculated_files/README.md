# calculated_files

Thư mục chứa **file đầu ra**: các file Excel kết quả do server tự động tính toán và commit. Không upload hay sửa tay vào thư mục này.

## File sẽ xuất hiện ở đây

Mỗi lần server xử lý một file trong [origin_files/](../origin_files/README.md) sẽ sinh ra một file mới:

```
<tên file gốc>_<YYYYMMDD_HHMMSS>.xlsx
```

Ví dụ, từ file gốc `Thuypn2_2026_Ban tin dau thau_VCS_11112026.xlsx`, chạy lúc 10:30:15 ngày 16/11/2026 sẽ cho:

```
Thuypn2_2026_Ban tin dau thau_VCS_11112026_20261116_103015.xlsx
```

- Hậu tố là thời điểm **bắt đầu chạy**, theo giờ Việt Nam (UTC+7).
- Cùng một file gốc chạy nhiều lần sẽ cho nhiều file kết quả khác nhau. Kết quả mới hơn có hậu tố lớn hơn và cập nhật trạng thái TBMT mới nhất từ muasamcong.
- File kết quả không ghi đè lên bản cũ, nên có thể so sánh giữa các lần chạy.

## Nội dung mỗi file

| Sheet | Nội dung |
|---|---|
| `Muasamcong` | Sheet gốc của bản tin, giữ nguyên |
| `muasamcong_refactor` | Dữ liệu đã làm sạch từ năm 2023, mỗi TBMT một dòng, kèm trạng thái, nhà thầu trúng, mã kế hoạch và các ghi chú |
| `Gói ATTT` | Các gói thầu thuộc lĩnh vực An toàn thông tin và trạng thái đã có nhà thầu hay chưa |
| `Kế hoạch chủ đầu tư` | Theo năm và chủ đầu tư: tổng tiền kế hoạch ATTT, dự toán còn lại, chênh lệch, ghi chú giải thích số âm |
| `Nhà thầu trúng thầu` | Theo năm và nhà thầu: số gói và tổng giá trúng |

Các số tổng hợp là công thức Excel tham chiếu về sheet dữ liệu. Hãy mở bằng **Microsoft Excel** (hoặc phần mềm bảng tính tương đương). Trình xem trên GitHub và chế độ Protected View chưa tính công thức nên các ô này có thể trống.

Giải thích chi tiết từng cột xem `HUONG_DAN_SU_DUNG.md` của script.

## Cách đọc nhanh

- Muốn xem **kết quả mới nhất**: mở file có hậu tố thời gian lớn nhất.
- Dòng có nhãn `Đang xét thầu`: TBMT chưa có kết quả trên muasamcong tại thời điểm chạy.
- Dòng có nhãn `Huỷ TBMT`: không tính vào tổng tiền kế hoạch của chủ đầu tư.
- Số âm ở sheet `Kế hoạch chủ đầu tư`: xem cột Ghi chú. Nguyên nhân thường gặp là không tra cứu được kế hoạch, hoặc giá bản tin cao hơn giá kế hoạch.

## Quy ước

- Server là bên duy nhất ghi vào thư mục này. Chỉ commit khi lần chạy hoàn tất thành công, không commit file dở dang.
- Không đặt file token, cache JSON hay file cấu hình vào đây.
- Thư mục sẽ lớn dần theo thời gian (mỗi file khoảng 1 đến 2 MB). Khi cần, có thể dọn các bản cũ và chỉ giữ một số bản gần nhất.

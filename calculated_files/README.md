# calculated_files

Đây là nơi **lưu các file kết quả**. Hệ thống tự tạo và thêm file vào đây, bạn không cần tải lên hay sửa gì.

## Trong thư mục này có gì

Mỗi lần hệ thống xử lý một bản tin ở thư mục [origin_files](../origin_files/README.md), nó tạo ra một file Excel kết quả mới. Tên file giống file gốc, thêm **ngày và giờ lúc bắt đầu chạy** ở cuối:

> `<tên file gốc>_<năm tháng ngày>_<giờ phút giây>.xlsx`

Ví dụ: bản tin `Thuypn2_2026_Ban tin dau thau_VCS_11112026.xlsx` được xử lý lúc 10 giờ 30 phút 15 giây ngày 16/11/2026 sẽ cho ra file:

> `Thuypn2_2026_Ban tin dau thau_VCS_11112026_20261116_103015.xlsx`

Giờ ghi trong tên là giờ Việt Nam.

- Mỗi lần chạy tạo ra một file riêng và **không ghi đè** lên file cũ. Nhờ vậy bạn có thể so sánh kết quả giữa các lần chạy.
- Cùng một bản tin chạy lại sau vài ngày có thể cho kết quả khác, vì trạng thái các gói thầu trên trang Mua sắm công thay đổi theo thời gian.
- Muốn xem kết quả **mới nhất**, hãy mở file có ngày giờ lớn nhất trong tên.

Muốn được báo mỗi khi có file kết quả mới, xem mục "Cách nhận thông báo khi xong" trong [README chính](../README.md).

## Bên trong mỗi file kết quả

| Sheet | Nội dung |
|---|---|
| Muasamcong | Bản tin gốc, giữ nguyên |
| muasamcong_refactor | Dữ liệu đã làm sạch từ năm 2023 trở đi, mỗi thông báo mời thầu một dòng, kèm trạng thái và tên nhà thầu trúng |
| Gói ATTT | Các gói thầu thuộc lĩnh vực An toàn thông tin, cho biết đã có nhà thầu trúng hay chưa |
| Kế hoạch chủ đầu tư | Theo từng năm và từng chủ đầu tư: tổng tiền kế hoạch, số còn lại và ghi chú giải thích |
| Nhà thầu trúng thầu | Theo từng năm và từng nhà thầu: số gói và tổng giá trị đã trúng |

Hãy mở file bằng **Microsoft Excel**. Các con số tổng hợp do Excel tự tính khi mở. Nếu xem trước ngay trên trang web, các ô này có thể hiện trống.

## Cách đọc nhanh

- **Đang xét thầu**: gói này chưa có kết quả chọn nhà thầu tại thời điểm chạy.
- **Huỷ TBMT**: thông báo mời thầu đã bị huỷ, không tính vào tổng tiền kế hoạch của chủ đầu tư.
- **Số âm** trong sheet "Kế hoạch chủ đầu tư": xem cột Ghi chú bên cạnh. Thường là do không tìm thấy kế hoạch tương ứng, hoặc giá trong bản tin cao hơn giá trong kế hoạch.

## Lưu ý

- Chỉ hệ thống mới thêm file vào thư mục này. Bạn không nên tự thêm hoặc sửa file ở đây.
- Mỗi file nặng khoảng 1 đến 2 MB, nên thư mục sẽ lớn dần theo thời gian. Khi cần, có thể xoá bớt các file cũ và chỉ giữ vài file gần nhất.

# vcs-muasamcong

Dự án tính toán thông tin mua sắm công, dữ liệu được lấy từ trang public https://muasamcong.mof.gov.vn/

## Dự án này làm gì

Bạn đưa vào một **bản tin đấu thầu** (file Excel liệt kê các mã thông báo mời thầu). Hệ thống tự tra từng mã trên trang Mua sắm công của Bộ Tài chính, rồi trả về một **file Excel kết quả** cho biết:

- Mỗi thông báo mời thầu hiện đang ở trạng thái nào: đang xét thầu, đã huỷ, không có nhà thầu trúng, hay đã có nhà thầu trúng (kèm tên nhà thầu).
- Gói thầu đó thuộc kế hoạch lựa chọn nhà thầu nào.
- Gói thầu nào thuộc lĩnh vực **An toàn thông tin**.
- Với từng chủ đầu tư theo từng năm: tổng tiền kế hoạch cho các gói An toàn thông tin là bao nhiêu, và còn lại bao nhiêu chưa có nhà thầu trúng.
- Từng nhà thầu đã trúng bao nhiêu gói, với tổng giá trị bao nhiêu.

## Các thư mục trong dự án

| Thư mục | Chứa gì | Ai đưa file vào |
|---|---|---|
| [origin_files](origin_files/README.md) | Bản tin đấu thầu gốc (file đầu vào) | Bạn tự tải lên |
| [calculated_files](calculated_files/README.md) | File kết quả sau khi tính (file đầu ra) | Hệ thống tự thêm vào |
| config | File cài đặt mà hệ thống dùng khi chạy: khoá truy cập vào trang Mua sắm công và danh sách từ khoá để nhận biết gói thầu An toàn thông tin | Người quản trị chỉnh khi cần |

## Cách hoạt động

1. Bạn tải bản tin đấu thầu mới lên thư mục `origin_files`.
2. Hệ thống trên máy chủ tự kiểm tra định kỳ. Khi thấy có bản tin mới chưa xử lý, nó sẽ bắt đầu tra cứu.
3. Hệ thống lần lượt tra từng mã thông báo mời thầu trên trang Mua sắm công, rồi tổng hợp thành file kết quả.
4. File kết quả được lưu vào thư mục `calculated_files`. Tên file giống file gốc, thêm ngày giờ lúc bắt đầu chạy ở cuối.

Bạn không cần làm gì thêm sau bước 1. Muốn xem kết quả, mở thư mục `calculated_files` và lấy file có ngày giờ mới nhất.

## Cách nhận thông báo khi xong

Mỗi khi hệ thống xử lý xong một bản tin, nó đăng một **Release** trên GitHub, kèm sẵn file kết quả để tải về. Bạn nhận thông báo như sau (cần có tài khoản GitHub):

1. Mở trang dự án trên GitHub, bấm nút **Watch** ở góc trên bên phải.
2. Chọn **Custom**, tick vào **Releases**, rồi bấm **Apply**.

Từ đó, mỗi lần có kết quả mới bạn sẽ nhận:
- một email có đường dẫn tới trang tải file,
- thông báo trên chuông của GitHub và trên ứng dụng GitHub trên điện thoại.

Nếu không nhận được email, vào **Settings → Notifications** trong tài khoản GitHub của bạn và kiểm tra mục email đã được bật chưa.

Bạn cũng có thể không cần theo dõi, mà tự vào mục **Releases** của dự án để tải file kết quả mới nhất.

Lưu ý: nếu hệ thống gặp lỗi và không tạo được kết quả thì sẽ không có Release nào được đăng, nên cũng không có thông báo. Khi chờ lâu mà chưa thấy, hãy liên hệ người quản trị để kiểm tra.

## File kết quả có những gì

Mỗi file kết quả là một file Excel gồm nhiều trang tính (sheet):

| Sheet | Nội dung |
|---|---|
| Muasamcong | Bản tin gốc, giữ nguyên, không bị sửa |
| muasamcong_refactor | Dữ liệu đã được làm sạch: chỉ lấy từ năm 2023 trở đi, mỗi mã thông báo chỉ còn một dòng, kèm trạng thái và tên nhà thầu trúng |
| Gói ATTT | Danh sách các gói thầu thuộc lĩnh vực An toàn thông tin |
| Kế hoạch chủ đầu tư | Theo từng năm và từng chủ đầu tư: tổng tiền kế hoạch, số còn lại và ghi chú giải thích |
| Nhà thầu trúng thầu | Theo từng năm và từng nhà thầu: số gói và tổng giá trị đã trúng |

Các con số tổng hợp được Excel tự tính khi bạn mở file, nên hãy mở bằng Microsoft Excel. Nếu xem trước ngay trên trang web, các ô này có thể hiện trống.

Trong cột "Đơn vị trúng thầu", ngoài tên nhà thầu còn có các nhãn sau:

| Nhãn | Ý nghĩa |
|---|---|
| Đang xét thầu | Chưa có kết quả chọn nhà thầu |
| Không có nhà thầu trúng | Gói thầu đã kết thúc nhưng không chọn được nhà thầu |
| Huỷ TBMT | Thông báo mời thầu đã bị huỷ. Gói này không được tính vào tổng tiền kế hoạch của chủ đầu tư |

## Lưu ý về thông tin

- Bản tin đấu thầu và file kết quả có thông tin nội bộ về kế hoạch và dự toán. Hãy chắc chắn dự án này chỉ chia sẻ cho đúng người cần xem.
- Khi khoá truy cập vào trang Mua sắm công hết hạn, chỉ cần thay khoá mới trong file cài đặt ở thư mục `config`. Không cần chỉnh gì ở máy chủ.
- Cách chạy thủ công và giải thích chi tiết từng cột nằm trong tài liệu hướng dẫn đi kèm công cụ tra cứu (không nằm trong dự án này).

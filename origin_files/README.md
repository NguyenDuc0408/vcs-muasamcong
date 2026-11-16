# origin_files

Thư mục chứa **file đầu vào**: các bản tin đấu thầu gốc do người dùng upload. Server đọc thư mục này để quyết định file nào cần xử lý.

## File sẽ xuất hiện ở đây

- File Excel `.xlsx` bản tin đấu thầu, ví dụ: `Thuypn2_2026_Ban tin dau thau_VCS_11112026.xlsx`.
- Mỗi lần có bản tin mới, upload thêm một file. Không cần xoá các file cũ.

## Yêu cầu về nội dung file

- Định dạng `.xlsx`. Sheet đầu tiên là sheet bản tin, tên mặc định `Muasamcong`.
- Hàng 1 là hàng tiêu đề.
- Có cột **Số TBMT** (mã thông báo mời thầu, dạng `IB2400xxxxx`). Script dựa vào cột này để tra cứu trên muasamcong.
- Có các cột ngày đăng, giá gói thầu, chủ đầu tư và tên gói thầu như bản tin hiện tại. Công cụ sẽ tự thêm các cột kết quả, nên **không cần** chuẩn bị trước.

## Quy ước

- **Không sửa file đã upload.** Khi cần chỉnh nội dung, upload thành một file mới (tên khác hoặc phiên bản mới), vì server xác định file mới nhất theo thời điểm commit.
- Tên file nên có ngày tháng để dễ phân biệt, không dùng ký tự đặc biệt như `:` hoặc `/`.
- Không đặt file kết quả vào thư mục này. File kết quả nằm ở [calculated_files/](../calculated_files/README.md).
- Server chỉ **đọc** thư mục này, không ghi vào đây.

## Cách server chọn file để xử lý

1. `git pull` định kỳ.
2. Tìm file `.xlsx` có thời điểm commit mới nhất.
3. Nếu file đó chưa từng được xử lý thì chạy script. Nếu đã xử lý rồi thì bỏ qua.

## Lưu ý bảo mật

Bản tin chứa thông tin nội bộ về kế hoạch đấu thầu. Hãy đảm bảo repo ở chế độ private trước khi upload dữ liệu thật.

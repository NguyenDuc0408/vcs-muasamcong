# origin_files

Đây là nơi bạn **tải bản tin đấu thầu lên**. Hệ thống sẽ đọc các file trong thư mục này để tính toán.

## Trong thư mục này có gì

Các file Excel (đuôi `.xlsx`) là bản tin đấu thầu, ví dụ: `Thuypn2_2026_Ban tin dau thau_VCS_11112026.xlsx`.

Mỗi khi có bản tin mới, bạn tải thêm một file. Không cần xoá các file cũ.

## File cần có gì bên trong

- Là file Excel, bản tin nằm ở trang tính đầu tiên.
- Dòng đầu tiên là dòng tiêu đề các cột.
- Có cột **Số TBMT** (mã thông báo mời thầu, ví dụ `IB2400123456`). Hệ thống dựa vào cột này để tra cứu.
- Có các cột thông tin như bản tin hiện tại: ngày đăng, giá gói thầu, chủ đầu tư, tên gói thầu.

Bạn không cần tự thêm các cột kết quả. Hệ thống sẽ tự bổ sung vào file kết quả.

## Quy ước khi tải file

- **Không sửa lại file đã tải lên.** Nếu cần chỉnh nội dung, hãy tải lên một file mới với tên khác hoặc phiên bản mới. Hệ thống luôn xử lý file được tải lên **gần nhất**.
- Nên có ngày tháng trong tên file để dễ phân biệt.
- Tên file không nên dùng các ký tự đặc biệt như dấu hai chấm hoặc dấu gạch chéo.
- Không để file kết quả vào đây. Kết quả nằm ở thư mục [calculated_files](../calculated_files/README.md).

## Sau khi tải lên thì sao

Hệ thống kiểm tra định kỳ. Khi thấy file mới, nó sẽ tự tra cứu rồi lưu kết quả sang thư mục `calculated_files`. File đã được xử lý rồi sẽ không bị xử lý lại, trừ khi người quản trị yêu cầu.

## Lưu ý

Bản tin có thông tin nội bộ về kế hoạch đấu thầu. Hãy chắc chắn dự án chỉ được chia sẻ cho đúng người trước khi tải dữ liệu thật lên.

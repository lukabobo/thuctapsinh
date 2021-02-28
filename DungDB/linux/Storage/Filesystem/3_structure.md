## Kiến trúc FS

File system bao gồm 2 hoặc 3 lớp. Đôi khi các lớp được chia rõ ràng, đôi lúc được kết hợp lại.

- Lớp thứ nhất – “logical file system”: “logical file system” chịu trách nhiệm tương tác với ứng dụng người dùng. Nó cung cấp API cho các hoạt động cơ bản – Mở, đóng, đọc, .. và truyền các yêu cầu xuống lớp dưới cho việc xử lý. “Logical file system” quản lý hoạt động mở đối tượng “file table” và “per-process file descriptors”. Lớp này cung cấp “file access, directory operations, và security and protection”.
- Lớp thứ 2 (không bắt buộc) - virtual file system. Là lớp giao diện cho phép hỗ trợ đồng thời nhiều loại file system vật lý, còn được gọi là thực thi file system.
- Lớp thứ 3 - physical file system. Đây là lớp liên quan đến hoạt động vật lý của thiết bị lưu trữ (disk). Nó xử lý các khối vật lý cho việc đọc hoặc ghi. Nó xử lý các buffer, memory management và chịu trách nhiệm bố trí các khối vật lý trong những ví trí được chỉ định. Physical file system tương tác với device drivers hoặc các kênh tới thiết bị lưu trữ.

File system chịu trách nhiệm tổ chức file và directories, chỉ định các phân vùng để lưu trữ hay không lưu trữ. Hiện tượng dư thừa các không gian lưu trữ mà không thể tận dụng được gọi là “slack space”. Độ lớn cấp phát cho các đối tượng được chọn khi file system được tạo. Việc chọn độ lớn cấp phát phụ thuộc vào độ lớn trung bình mà file cần cho việc lưu trữ. Thông thướng sẽ cấp phát hợp lý nhất cho việc lưu trữ. Khái niệm phân mảnh xảy ra khi các không gian dư thừa, không lữu trữ hoặc các file đơn không tiếp giáp. Việc này xuất hiện trong quá trình sử dụng, tạo, sửa, xóa file. Khi file được tạo mới cần không gian lớn hơn các phân mảnh hiện có(hợp lý nhất cho việc lưu trữ). Các file được xóa nhưng không gian đang sử dụng lại không được cấp phát cho việc lưu trữ

- Filename Filename được sử dụng để xác thực vị trí lưu trữ trong file system. Hầu hết file system có hạn chế về độ dài, quy tắc đặt tên “filename”.

- Directories Filesystem còn có dạng directory, cho phép user nhóm các file riêng lẻ thành 1 tập hợp. Có thể được thực hiện bằng cách gán file với với số thứ tự trong table of content hoặc inode trong hệ điều hành nhân Unix. Directory có thể cho phép tổ chức dạng phân cấp chứ nhiều file và direc bên trong.

- Metadata Thông tin đi kèm với mỗi file system. Như độ dài của dữ liệu file được lưu với số block được phân phát hoặc dung lượng. Thời gian chỉnh sửa cuối cùng. Thời gian tạo, khối, character, socket, .. Chủ sở hữu userID và group ID, quyển truy cập
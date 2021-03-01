# Truy xuất và kiểm soát truy xuất

## Truy #xuất filesystem

Các thông tin này cần được truy xuất và đọc vào bộ nhớ máy tính để xử lý. Thông tin trong tập tin có thể được truy xuất bằng nhiều cách.

### Truy xuất tuần tự

- Phương pháp đơn giản nhất, Thông tin trong tập tin được xử lý có thứ tự
- Chế độ truy xuất này là thông dụng nhất. VD: bộ soạn thảo và biên dịch

### Truy xuất trực tiếp

- Truy xuất trực tiếp (hay truy xuất tương đối)
- Tập tin được hình thành từ các logical records có chiều dài không đổi
- Cho phép người lập trình đọc và viết các mẫu tin nhanh chóng, không theo thứ tự
- Để truy xuất trực tiếp, tập tin được hiển thị như một chuỗi các khối hay mẫu tin được đánh số
- Phương pháp được sử dụng truy xuất tức thời lượng lớn thông tin (pp cơ sở dữ liệu).

### Các phương pháp truy xuất khác

- Được xây dựng trên cơ sở của phương pháp truy xuất trực tiếp
- Việc xây dựng chỉ mục cho tập tin. Chỉ mục chứa các con trỏ chỉ tới các khối khác
- Để tìm một mẫu tin trong tập tin - tìm chỉ mục - con trỏ để truy xuất tập tin trực tiếp và tìm mẫu tin mong muốn

## Kiểm soát truy xuất

Truy xuất dựa trên định danh người dùng. Cơ chế thông dụng nhất để cài đặt truy xuất phụ thuộc định danh là gắn với mỗi tập tin và thư mục một danh sách kiểm soát truy xuất (access-control list-ACL) xác định tên người dùng và kiểu truy xuất được phép cho mỗi người dùng

Khi một người dùng yêu cầu truy xuất tới một tập tin cụ thể, hệ điều hành kiểm tra danh sách truy xuất được gắn tới tập tin đó. Nếu người dùng đó được liệt kê cho truy xuất được yêu cầu, truy xuất được phép. Ngược lại, sự vi phạm bảo vệ xảy ra và công việc của người dùng đó bị từ chối truy xuất tới tập tin.

Để tạo được danh sách, hệ thống phân loại người dùng theo mỗi tập tin:

- Người sở hữu (Owner): người dùng tạo ra tập tin đó
- Nhóm (Group): tập hợp người dùng đang chia sẻ tập tin và cần truy xuất tương tự là nhóm hay nhóm làm việc
- Người dùng khác (universe): tất cả người dùng còn lại trong hệ thống

Để tạo và cài đặt danh sách, trên Unix chỉ có người quản trị mới có quyền tạo.
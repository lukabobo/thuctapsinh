# Tips and tricks

## CPU single-core utilization

Giám sát toàn bộ lượng sử dụng CPU là rất tốt nhưng không thể phát hiện ra toàn bộ vấn đề gặp phải. Ví dụ như 1 trong 16 CPU hoạt động liên tục trong thời gian dài.

Để giải quyết vấn đề này, checkmk cung cấp khả năng giám sát từng CPU. `Levels over extended periods on a single core CPU utilization`

![Imgur](https://i.imgur.com/gWvbtKF.png)

## Giám sát các dịch vụ trên Windows

Checkmk không tự động giám sát các dịch vụ trên Windows vì nó không biết dịch vụ nào cần thiết để giám sát. Nếu bạn không muốn tốn công đặt nhiều rule cho nhiều dịch vụ thì bạn chỉ cần check xem tất cả các dịch vụ có khởi động cùng hệ thống hay không. Để làm được bạn cần set rule giám sát trong phần Service states. 

![Imgur](https://i.imgur.com/VOzxYc1.png)

- Dịch vụ nào khởi động cùng hệ thống mà đang chạy thì OK
- Dịch vụ nào khởi động cùng hệ thống mà đang tắt thì CRIT
- Dịch vụ nào có yêu cầu khởi động cùng hệ thống mà đang chạy WARN

Nhưng rule này chỉ áp dụng với các dịch vụ đã được giám sát. Vì thế chúng ta cần tạo rule mới trong Windows Service Discovery. Chỗ này checkmk sẽ tự động đề nghị xem dịch vụ nào cần được giám sát.

Để đặt rule chỗ này đầu tiên ta đặt dấu `.*` tại phần Services (Regular Expressions). Tất cả các dịch vụ sẽ xuất hiện. Xem xét các dịch vụ cần được giám sát và quya trở lại phần rule và lọc theo ý muốn. Ví dụ như trong hình dưới:

![Imgur](https://i.imgur.com/m9u4mRP.png)

Sau đó ta click nút Automatic refresh để làm mới danh sách dịch vụ được giám sát.

## Giám sát host thường có trạng thái là DOWN

Khi máy cần giám sát là máy in, chúng thường có trạng thái down. Ta tạo một rule như sau:

![Imgur](https://i.imgur.com/3NxNWfr.png)

![Imgur](https://i.imgur.com/WpZCOSM.png)

Nếu host down checkmk vẫn xem đó là trạng thái bình thường.

Để tránh cảnh báo khi timeout, ta thêm rule vào `Status of the Check_MK services`

![Imgur](https://i.imgur.com/htPRZlu.png)

Chọn `State in case of connection problems` và `State in case of a timeout` là OK

## Đặt ngưỡng cảnh báo sử dụng disk

Ngưỡng cảnh báo mặc định là 80% sẽ warning. Nhưng đối với các server có ổ cứng lớn, ví dụ như 2T, sử dụng hết 80% thì vẫn còn 400GB. Như thế vẫn là rất nhiều để cảnh báo.

Tại `Filesystems (used space and growth)` apply 1 rule. Ngưỡng mặc định. Ta bật `Magic factor (automatic level adaptation for large filesystem)`, và thêm vào số 0.8. Bật `Reference size for magic factor` và nhập 100GB

![Imgur](https://i.imgur.com/kDFukSt.png)

Bây giờ máy có ổ cứng bằng 100GB sẽ có ngưỡng mặc định

Máy có ổ lớn hơn 100GB sẽ có ngưỡng cao hơn

Máy có ổ nhỏ hơn 100GB sẽ có ngưỡng thấp hơn

![Imgur](https://i.imgur.com/pio5Fd3.png)

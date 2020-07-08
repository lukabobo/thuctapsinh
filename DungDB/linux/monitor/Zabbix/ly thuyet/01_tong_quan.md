# Tổng quan về Zabbix

## Giới thiệu

Zabbix được tạo ra bởi Alexei Vladishev và đang được phát triển bởi zabbix SIA. Đây là giải pháp giám sát mã nguồn mở. Có khả năng giám sát hệ các thông số trong hệ thống mạng. Nó có thể cài đặt để gửi các thông báo qua nhiều kênh để đến với người quản trị nhằm khắc phục nhanh nhất các sự cố.

Zabbix cũng support web-based để ta có thể theo dõi trạng thái của các thiết bị. Điều này tạo điều kiện dễ dàng cho việc giám sát khi bạn ở bất kỳ đâu.

## Ưu điểm của Zabbix
- Giám sát cả Server và thiết bị mạng
- Dễ dàng thao tác và cấu hình
- Hỗ trợ máy chủ Linux, Solaris, FreeBSD …
- Đáng tin cậy trong việc chứng thực người dùng
- Linh hoạt trong việc phân quyền người dùng
- Giao diện web đẹp mắt
- Thông báo sự cố qua email và SMS
- Biểu đổ theo dõi và báo cáo
- Mã nguồn mở và chi phí thấp

## Nhược điểm

- Không có giao diện web mobile hỗ trợ.
- Không phù hợp với hệ thống mạng lớn hơn 1000+ node thiết bị client cần giám sát. Lúc này phát sinh vấn đề hiệu suất về PHP và Database.
- Thiết kế template/alerting rule đôi khi khá phức tạp.

## Các thành phần cơ bản của Zabbix
- Zabbix server
Đây là thành phần trung tâm của phần mềm Zabbix. Zabbix Server có thể kiểm tra các dịch vụ mạng từ xa thông qua các báo cáo của Agent gửi về cho Zabbix Server và từ đó nó sẽ lưu trữ tất cả các cấu hình cũng như là các số liệu thống kê.

- Zabbix Proxy
Là phần tùy chọn của Zabbix. Nó có nhiệm vụ thu nhận dữ liệu, lưu trong bộ nhớ đệm và chuyển đến Zabbix Server.
Zabbix Proxy là một giải pháp lý tưởng cho việc giám sát tập trung của các địa điểm từ xa, chi nhánh công ty, các mạng lưới không có quản trị viên nội bộ.
Zabbix Proxy cũng được sử dụng để phân phối tải của một Zabbix Server

- Zabbix Agent
Để giám sát chủ động các thiết bị cục bộ và các ứng dụng (ổ cứng, bộ nhớ, …) trên hệ thống mạng. Zabbix Agent sẽ được cài lên trên Server và từ đó Agent sẽ thu thập thông tin hoạt động từ Server mà nó đang chạy và báo cáo dữ liệu này đến Zabbix Server để xử lý.

- Web interface
Để dễ dàng truy cập dữ liệu theo dõi và sau đó cấu hình từ giao diện web cung cấp. Giao diện là một phần của Zabbix Server, và thường chạy trên các máy chủ.

## Tham khảo

https://github.com/niemdinhtrong/thuctapsinh/tree/master/NiemDT/Ghichep-zabbix
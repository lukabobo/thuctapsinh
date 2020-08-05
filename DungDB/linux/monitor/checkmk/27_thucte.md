# Note thực tế

sử dụng agent và active check ICMP đối với các host cần giám sát. Gặp hiện tượng một loạt các host có cảnh báo down xong up lại ngay trong 1 phút sau.

## Hướng giải quyết:

Thêm các gateway đến các host đang giám sát, chỉ kiểm tra bằng active check PING.

Kiểm tra mạng lúc đó có vấn đề gì không (sử dụng smokeping) hoặc ping ngay vào host bị down để xem có phải bị down thật hay không. Nếu là báo động giả, cần xem lại mức độ sử dụng tài nguyên của server checkmk bằng lệnh htop. Nếu thấy cứ sau một khoảng thời gian ngắn, CPU lại bị chiếm dụng 100% mà các tiến trình đều là của checkmk. Cần xem xét nâng CPU cho server. 

Nếu các hiện tượng kia vẫn diễn ra.

- Nâng `Agent TCP connect timeout` lên 1 phút.

- `Normal check interval for service checks` của các dịch vụ PING, Discovery và Check_MK lên 2 phút. Và làm tương tự với `Normal check interval for host checks`, `Retry check interval for host checks`, `Retry check interval for service checks`

- Tăng interval HW/SW inventory `Normal check interval for service checks` `Check_MK HW/SW Inventory$` lên 1 ngày.

Đối với các host vẫn có hiện tượng mất gói tin và có cảnh báo. Cần xem host đó có sử dung csf hay không. Nếu có thì cần allow IP server checkmk bên trong host đó.

Nếu hiện tượng kia vẫn còn thì ta sử dụng delay cảnh báo lại 2 phút, nếu trong 2 phút mà host, service đó up lại thì sẽ không gửi cảnh báo

`Delay host notifications` và  `Delay service notifications`


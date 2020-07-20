# Cấu hình dịch vụ

https://checkmk.com/cms_intro.html

Như đã nói ở phần 2:

Service: một dịch vụ có thể là bất cứ thứ gì. Dịch vụ là một phần ảnh hưởng đến việc host có OK hay không. State của dịch vụ thường được xác định chỉ khi host trong điều kiện UP. Các trạng thái của dịch vụ là OK (ok), WARN (cảnh báo), CRIT (nghiêm trọng), UNKNOWN (không biết).

Ví dụ một host có thể có các dịch vụ memory (RAM), CPU load, CPU utilization, 	Filesystem /, vân vân.

Bên trong một dịch vụ thì có nhiều Metric khác nhau. Ví dụ trong dịch vụ memory thì có RAM install, RAM used, SWAP installed, SWAP used, Free RAM, Free SWAP, vân vân.

Vd các service trong một host

![Imgur](https://i.imgur.com/O81gilX.png)

Đối với mỗi dịch vụ này, về nguyên tắc có ba khả năng:

- Undecided: Bạn chưa quyết định có giám sát dịch vụ này hay không
- Monitored: Dịch vụ đang được giám sát
- Disabled: Không giám sát dịch vụ này

Ban đầu, tất cả các dịch vụ đều là undecided. Click vào nút `Fix all missing/vanished`. 
 
Sau khi chỉnh sửa, click vào nút `Activate affected` sẽ thấy các dịch vụ được giám sát. 

## Metrics

Ví dụ về Metric của dịch vụ File system /

![Imgur](https://i.imgur.com/E0KaJx0.png)

Con số cần chú ý là 22.05 phần trăm đã sử dụng. Đây là tông quan. Để biết thêm chi tiết thì click vào dịch vụ

![Imgur](https://i.imgur.com/5728MwG.png)

Các biểu đồ trong phiên bản RAW edition không thể tương tác được. 

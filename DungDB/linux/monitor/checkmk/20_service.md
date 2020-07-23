# Dịch vụ và Metrics

https://checkmk.com/cms_intro.html

Như đã nói ở phần 2:

Service: một dịch vụ có thể là bất cứ thứ gì. Dịch vụ là một phần ảnh hưởng đến việc host có OK hay không. State của dịch vụ thường được xác định chỉ khi host trong điều kiện UP. Các trạng thái của dịch vụ là OK (ok), WARN (cảnh báo), CRIT (nghiêm trọng), UNKNOWN (không biết).

Các trạng thái dịch vụ:

![Imgur](https://i.imgur.com/nrxwwS7.png)

các trạng thái của dịch vụ sắp xếp theom ức độ nghiêm trọng tăng dần:

![Imgur](https://i.imgur.com/8CjpijL.png)

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

Các icon quan trọng:

![Imgur](https://i.imgur.com/p8LkO8a.png)

Ví dụ về Metric của dịch vụ `Filesystem /`

![Imgur](https://i.imgur.com/E0KaJx0.png)

Con số cần chú ý là 22.05 phần trăm đã sử dụng. Đây là tông quan. Để biết thêm chi tiết thì click vào dịch vụ

![Imgur](https://i.imgur.com/5728MwG.png)

Các biểu đồ trong phiên bản RAW edition không thể tương tác được. 

## Đọc thêm

https://checkmk.com/cms_monitoring_basics.html

https://checkmk.com/cms_wato_services.html#checkplugins
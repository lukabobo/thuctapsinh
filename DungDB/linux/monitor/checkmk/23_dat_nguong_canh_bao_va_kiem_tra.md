# Đặt ngưỡng cảnh báo cho các service `Memory`, `CPU utilization` và `Filesystem /` và test cảnh báo

## Đặt ngưỡng cảnh báo RAM

Click vào dấu 3 gạch tại dịch vụ `Memory` trong host cần giám sát. Chọn `Parameter for this device`

![Imgur](https://i.imgur.com/frNt5rx.png)

Chọn `Memory and Swap usage on Linux`

![Imgur](https://i.imgur.com/1w0Bxe2.png)

Tạo rule, có thể chọn chỉ áp dụng cho host này hoặc có thể áp dụng cho cả folder

Chọn các ngưỡng cảnh báo, có thể chọn nhiều thông số. Ở đây tôi chỉ chọn `Levels for RAM`. Khi RAM sử dụng trên 70% sẽ là trạng thái WARN. Trên 90% sẽ là trạng thái CRIT

![Imgur](https://i.imgur.com/siHu6dm.png)

Lưu lại và áp dụng thay đổi.

Tiếp theo sang phần `Notification` tạo rule gửi cảnh báo.

Ở đây tôi sẽ tạo 1 rule gửi cảnh báo qua email. Gửi đến user dungz1207. Trong phần `Conditions`, chọn đúng folder, chọn đúng service (Memory). Ở đây tôi chọn luôn các service khác là CPU utilization và Filesystem /

![Imgur](https://i.imgur.com/uXGtqQ9.png)

![Imgur](https://i.imgur.com/YpMbfHv.png)

Và chọn các trạng thái gửi cảnh báo. Chỗ này bạn nên chọn tùy theo nhu cầu của mình.

![Imgur](https://i.imgur.com/wlsHtlo.png)

Lưu lại và áp dụng thay đổi.

Vào host đang được giám sát có IP là 10.10.10.115. Host này là VPS chạy Centos, 2GiB RAM, 2 core, 30GiB disk.

Tăng mức sử dụng RAM với stress.

    stress -m 1 --vm-bytes 2G

Lúc này lượng RAM sử dụng sẽ tăng lên 

![Imgur](https://i.imgur.com/OqLrAF8.png)

Kết quả cảnh báo RAM:

![Imgur](https://i.imgur.com/p99Ahgu.png)

Tương tự tạo cảnh báo với Telegram.

Kết quả nhận được:

![Imgur](https://i.imgur.com/tci8ore.png)

## Đặt ngưỡng cảnh báo mức sử dụng CPU 

Ở đây tôi đặt rule WARN khi vượt 50%, CRIT khi vượt 60% trong 30s liên tục

Ở phần Notifications làm giống như ở phần đặt ngưỡng cảnh báo RAM.

Tăng mức sử dụng CPU với lệnh stress.

    stress -c 2

Mức sử dụng CPU bị đẩy lên cao

![Imgur](https://i.imgur.com/ZsiFlhe.png)

Kết quả cảnh báo qua email:

![Imgur](https://i.imgur.com/D3WP8IK.png)

Kết quả cảnh báo qua telegram

![Imgur](https://i.imgur.com/M0HEogl.png)

## Đặt ngưỡng cảnh báo cho Filesystem /

Để cho dễ test, tôi đặt ngưỡng 6% sẽ là WARN và 10% là CRIT

![Imgur](https://i.imgur.com/m0FioKC.png)

Ở phần Notifications làm giống như ở phần đặt ngưỡng cảnh báo RAM.

Dùng stress tạo một file cos dung lượng 2 GiB

    stress -d 1 --hdd-bytes 2G -t 1m

Kết quả cảnh báo mail

![Imgur](https://i.imgur.com/i6qYfAy.png)

Kết quả cảnh báo qua telegram

![Imgur](https://i.imgur.com/PyGoSc7.png)

## Đọc thêm: 

https://checkmk.com/cms_notifications.html
# Giám sát chứng chỉ SSL

Tạo một web server đơn giản, cài SSL Let's Enscrypt.

Thông tin chứng chỉ

![Imgur](https://i.imgur.com/5wpQT54.png)

Ta thực hiện giám sát host này với plugin như ở đây: https://github.com/lukabobo/thuctapsinh/blob/master/DungDB/linux/monitor/checkmk/10_monitor_apache.md

Kết quả: 

![Imgur](https://i.imgur.com/tgrvwh2.png)

Tiếp theo ta tạo rule áp dụng cho host này để xem thời hạn còn lại của chứng chỉ

![Imgur](https://i.imgur.com/Rzc5iWQ.png)

Vào `Ative checks` -> `Check connecting to a TCP port`

Tạo rule như sau:

![Imgur](https://i.imgur.com/xFVC2vr.png)

Khi chứng chỉ còn 30 ngày nữa sẽ hết hạn thì sẽ có trạng thái WARN, sẽ có thông báo đến người quản trị. Khi còn 10 ngày sẽ có trạng thái CRIT.

Sau đó áp dụng thay đổi

Kết quả:

![Imgur](https://i.imgur.com/y53CaGp.png)
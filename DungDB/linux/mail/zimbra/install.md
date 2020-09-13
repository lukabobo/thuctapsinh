# Cài đặt mail zimbra

Trước khi cài kiểm tra có dịch vụ gì đang chạy không bằng lệnh

    netstat -tunlp

Stop các dịch vụ httpd, postfix, sendmail,firewall, iptable

Tải ver 8.8.12
https://zimbra.org/download/zimbra-collaboration/8.8.12

    wget https://files.zimbra.com/downloads/8.8.12_GA/zcs-8.8.12_GA_3794.RHEL7_64.20190329045002.tgz

    tar -xvf zcs-8.8.12_GA_.....
    cd zcs-8.8.12_GA_.....
    ./install.sh

Option này chọn `N`

`Install zimbra-dnscache [Y] N`

Còn lại chọn `Y` ở tất cả các option khác

Nếu đã trỏ bản ghi rồi thì 

![Imgur](https://i.imgur.com/NuAYCde.png)

Nếu chưa trỏ bản ghi thì 

![Imgur](https://i.imgur.com/rIGExWq.png)

Nếu có dịch vụ nào đang stop thì dùng cấu trúc

    za(tên-service)ctl start

Ví dụ

    zmamtctl start

## Tips

Xử lý tài khoản spam

Vào phần `Quản lý tài khoản`. Chọn `Đã đóng`

Bắt tài khoản spam mail

Vào **Giám sát** - hàng thư đợi - thư đã gửi

Xem dung lượng

Vào Giám sát

Xem dung lượng

Nâng dung lượng file đính kèm

    postconf -e 'message_size_limit = 104857600'
    zmprov mcf zimbraMtaMaxMessageSize 50000000
    zmprov mcf zimbraFileUploadMaxSize 50000000
    zmprov mcf zimbraMailContentMaxSize 104857600

    postfix reload
    zmcontrol restart

Kiểm tra thư không nhận được. Vào phần forward xem có tick phần xóa thư local không. Nếu có thì bật lên

Cấu hình thiết lập chung ASAV. Bỏ tick chặn các tài liệu. lưu và restart dịch vụ antivirus 


## LOG

/var/log/zimbra.log

Gửi thành công:

    status 250 2.0.0 
    removed

Nếu khác tức là chưa gửi được



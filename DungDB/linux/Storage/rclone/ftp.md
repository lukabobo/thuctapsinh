# Kết nối rclone tới FPT server

Mô hình

![Imgur](https://i.imgur.com/uAoiosC.png)

## Cấu hình FTP server 

Thao tác trên máy 10.10.34.175

Cài đặt ftp server 

    yum install vsftpd -y

Bật dịch vụ

    systemctl start vsftpd
    systemctl enable vsftpd

Cấu hình firewall

    firewall-cmd --permanent --add-port=21/tcp
    firewall-cmd --permanent --add-service=ftp
    firewall-cmd --reload

Cấu hình

    vi /etc/vsftpd/vsftpd.conf

Sửa các dòng

    anonymous_enable=NO    // Không cho kết nối nặc danh 
    local_enable=YES        // Cho phép kết nối cục bộ
    write_enable=YES        //Cho phép người dùng nội bộ tải lên

    chroot_local_user=YES
    allow_writeable_chroot=YES
    chroot_list_enable=YES
    chroot_list_file=/etc/vsftpd.chroot_list

    ftpd_banner="Welcome FTP Server"

    pasv_min_port=30000
    pasv_max_port=31000

    userlist_enable=YES
    userlist_file=/etc/vsftpd/user_list
    userlist_deny=NO

    local_root=<đường_dẫn_thư_mục>  # ta có thể chỉ định thư mục home khi người dùng đăng nhập vào hệ thống

Xem ý nghĩa các dòng tại đây: https://news.cloud365.vn/ftp-huong-dan-cau-hinh-ftp-server-tren-centos-7-voi-vsftpd/

Khởi động lại dịch vụ 

    systemctl restart vsftpd

Thêm cấu hình firewall

    firewall-cmd --permanent --add-port=30000-31000/tcp
    firewall-cmd --reload

Tạo user mới

    useradd cloud365
    passwd cloud365

Nhập password cho user

Thêm user đó vào file `/etc/vsftpd.chroot_list`

    vi /etc/vsftpd.chroot_list

![Imgur](https://i.imgur.com/Ir6Qd6H.png)

## Kết nối rclone từ client 10.10.34.179 đến FTP server 

Thao tác trên máy 10.10.34.179

    rclone config

Nhấn nút `n` đế tạo kết nối mới

Đặt tên tùy ý bạn cho kết nối

Nhấn số 11 để tạo kết nối FTP

![Imgur](https://i.imgur.com/kOJegBT.png)

Chọn host. Chỗ này gõ đúng IP của FTP server. Ở đây tôi nhập 10.10.34.175

Gõ tên user ta dùng để đăng nhập. Nhập tên user ta vừa tạo lúc nãy là cloud365

Các option khác để mặc định (Nhấp Enter)

![Imgur](https://i.imgur.com/q9ymnjW.png)

Như vậy chúng ta đã kết nối được rclone với FTP server

Tất cả các file share đều nằm ở thư mục đã chỉ định ở dòng `local_root=` trong file `/etc/vsftpd/vsftpd.conf`, nếu không chỉ định thì nó là thư mục này `/home/cloud365` (vì ta kết nối và đăng nhập bằng user cloud365)

## Test 

**Lưu ý:** Trước khi truyền thư mục/file từ client sang FPT server cần thêm quyền thực thi cho thư mục/file đó

Tôi tạo 1 thư mục tên dung ở client. Bên trong thư mục dung có các file dung.txt  test.txt và thư mục test

![Imgur](https://i.imgur.com/7VzTcYX.png)

    mkdir dung
    chmod +x dung
    rclone copy /root/dung ftp:

Kiểm tra ở FTP server

![Imgur](https://i.imgur.com/NeYoO2L.png)

Truyền file ngược lại từ FTP server sang client ta làm tương tự

Tạo một file nguoc.txt ở FTP server. Đứng tại client chạy lệnh

    rclone copy ftp: /root/dung

Kết quả:

![Imgur](https://i.imgur.com/c2EcbB4.png)


Đọc thêm các lệnh rclone tại đây: https://rclone.org/commands/

## Tham khảo 

https://www.tecmint.com/install-ftp-server-in-centos-7/

https://news.cloud365.vn/ftp-huong-dan-cau-hinh-ftp-server-tren-centos-7-voi-vsftpd/
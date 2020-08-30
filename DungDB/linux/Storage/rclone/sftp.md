# Kết nối rclone tới sFTP server 

sFTP (viết tắt của từ Secure File Transfer Protocol, hoặc SSH File Transfer Protocol) là một giao thức mạng giúp bạn có thể upload hoặc download dữ liệu trên máy chủ. Bạn cũng có thể sử dụng giao thức này để sửa, tạo hoặc xóa các tập tin và thư mục trên máy chủ Linux.

Bởi vì sFTP sử dụng giao thức SSH để kết nối nên các dữ liệu bạn di chuyển thông qua giao thức này cũng sẽ được mã hóa để bảo mật dữ liệu tốt hơn.

Mô hình:

SFTP server: 10.10.34.174 Centos 7

Client: 10.10.34.177

![Imgur](https://i.imgur.com/YnEYAIT.png)

## Cấu hình SFTP server 

Không giống như FTP thông thường, không cần cài đặt các gói bổ sung để sử dụng SFTP. Chúng chỉ yêu cầu gói sshd tạo sẵn đã được cài đặt trong quá trình cài đặt trên máy chủ. Do đó, chỉ cần kiểm tra để xác nhận xem bạn đã có gói SSH cần thiết hay chưa. Dưới đây là các bước:

Chạy lệnh:

    rpm -qa|grep ssh

Kết quả sẽ trông giống như sau:

    [root@localhost ~]# rpm -qa|grep ssh
    openssh-server-7.4p1-21.el7.x86_64
    openssh-clients-7.4p1-21.el7.x86_64
    libssh2-1.8.0-3.el7.x86_64
    openssh-7.4p1-21.el7.x86_64
    [root@localhost ~]#

Cấu hình giao thức SSH

    nano /etc/ssh/sshd_config

Ở đây tôi sẽ dùng user root để truy cập SFTP server. Sửa dòng

    PermitRootLogin yes

Thêm vào dòng sau ở cuối file:

    ForceCommand internal-sftp

Khởi động lại dịch vụ:

    service sshd restart

Nếu muốn sử dụng user khác để tăng tính bảo mật. Tham khảo hướng dẫn ở đây: https://www.howtoforge.com/tutorial/how-to-setup-an-sftp-server-on-centos/

## Test

Sang máy client

    sftp root@10.10.34.179

![Imgur](https://i.imgur.com/13BM4qX.png)

Tạo một file test ở server 

![Imgur](https://i.imgur.com/W91fYQ9.png)

Sang client kiểm tra. Đã thấy có file test

![Imgur](https://i.imgur.com/Mq5ba5k.png)

## Tạo kết nối rclone

Thao tác ở client

Tải rclone

    curl https://rclone.org/install.sh | sudo bash

Sau đó tạo kết nối theo các bước:

    rclone config

Nhập `n` để tạo kết nối mới

Nhập tên tùy ý. Ở đây tôi nhập tên là sftp để dễ phân biệt với các kết nối khác

Nhập `29` để tạo kết nối SSH/SFTP Connection "sftp"

![Imgur](https://i.imgur.com/EqUElS4.png)

Tiếp theo nhập host, đây chính là địa chỉ IP của SFTP server. Nhập vào 10.10.34.174 đối với trường hợp này

![Imgur](https://i.imgur.com/9tbe3qo.png)

Tiếp tục, nhập tên user đăng nhập sftp, port sử dụng, và password của user (nhấn y để nhập password của ta)

![Imgur](https://i.imgur.com/INIbsqw.png)

Các option khác là các option bảo mật, ở đây tôi sẽ để mặc định. Nhấn Enter đối với các option:

- key_pem>
- key_file>
- key_use_agent> 
- use_insecure_cipher>

Nhấn y (this is OK) để xác nhận tạo kết nối

![Imgur](https://i.imgur.com/S0jJnDe.png)

Vậy là kết nối đã được tạo.

### Test copy dữ liệu 2 phía.

Tạo một file test 1 phía sftp server. Sau đó sang phía client, copy toàn bộ dữ liệu phía server về

![Imgur](https://i.imgur.com/ogl1ad3.png)

Đứng ở phía client tạo một file test2

![Imgur](https://i.imgur.com/11nkLX5.png)

Kiểm tra phía server 

![Imgur](https://i.imgur.com/F6ahkLa.png)
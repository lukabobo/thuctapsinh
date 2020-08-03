# TCP wrapper

Cài đặt

    yum -y install tcp_wrappers

Sử dụng lệnh sau để biết dịch vụ có thể dùng với TCP wrapper hay không

    [root@dlp ~]# ldd /usr/sbin/sshd | grep wrap
        libwrap.so.0 => /lib64/libwrap.so.0 (0x00007f01b4e2a000)
    # Kết quả như sau tức là có thể dùng được vì nó có 'libwrap'

Đối với 1 VM có nhiều card mạng, trong đó có 1 card mạng public

Ta chỉ cho phép SSH qua IP local, không cho phép SSH qua IP pulic để tránh khả năng bị dò quét bằng cách dùng TCP wrapper.

Cấu hình không cho phép SSH từ bất cứ ai

    vi /etc/hosts.deny

Nhập vào nội dung

    sshd: ALL

Chỉ cho phép SSH từ dải mạng cụ thể

    vi /etc/hosts.allow

Nhập vào nội dung

    sshd: 192.168.34. #dải mạng được cho phép

## Tham khảo

https://www.server-world.info/en/note?os=CentOS_7&p=tcp_wrapper
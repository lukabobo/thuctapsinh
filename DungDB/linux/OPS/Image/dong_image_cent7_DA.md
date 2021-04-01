# Note lại cách đóng image Centos 7 + Directadmin

Tạo VPS có cấu hình:
CPU: 1 core 
RAM: 2G
Disk: 10G

Đầu tiên thực hiện hết bước 3

## Cài DA

Sau đó cài đặt DA theo hướng dẫn sau:
```
wget --user=nhanhoa --password=15935700 103.57.210.13/latest
chmod +x latest
./latest
```

Chờ script chạy xong. Khoảng 20-30 phút

### Build let’s encrypt trong directadmin 

Lưu ý: Let’s Encrypt chỉ hỗ trợ với version directadmin từ 1.5 trở lên

Bật tính năng Let’s Encrypt trong directadmin 

    echo "letsencrypt=1" >> /usr/local/directadmin/conf/directadmin.conf

Bật SNI trên DirectAdmin

    echo "enable_ssl_sni=1" >> /usr/local/directadmin/conf/directadmin.conf

Khời động lại dịch vụ DirectAdmin

    systemctl restart directadmin

Update license Let's Encrypt

    wget -O /usr/local/directadmin/scripts/letsencrypt.sh http://files.directadmin.com/services/all/letsencrypt.sh

Update web-server configs trên DirectAdmin

    cd /usr/local/directadmin/custombuild
    ./build letsencrypt
    ./build rewrite_confs

Cài đặt đa phiên bản php trong Directadmin

Trong hướng dẫn này sẽ build version php 5.6 và 7.4

    cd /usr/local/directadmin/custombuild

    ./build set php1_mode suphp
    ./build set php2_mode suphp
    ./build set php1_release 7.4
    ./build set php2_release 5.6

    ./build php n
    ./build rewrite_confs

### Cài CSF

    wget http://files.directadmin.com/services/all/csf/csf_install.sh

    /bin/sh ./csf_install.sh

Update và kiểm tra lại phiên bản

    csf -u
    csf -v

### Build Roundcube 

    cd /usr/local/directadmin/custombuild
    ./build roundcube

### Cài imunifyAV

    wget https://repo.imunify360.cloudlinux.com/defence360/imav-deploy.sh
    bash imav-deploy.sh
    yum update imunify-antivirus -y

### Check version Apache

    httpd -v

Phiên bản stable mới nhất hiện tại là 2.4.46

### Secure thư mục `/tmp`

    mount -t tmpfs -o defaults,nodev,nosuid,noexec tmpfs /tmp/
    mount -t tmpfs -o defaults,nodev,nosuid,noexec tmpfs /var/tmp/
    mount -t tmpfs -o defaults,nodev,nosuid,noexec tmpfs /dev/shm

    echo "tmpfs                   /tmp                    tmpfs   defaults,nodev,nosuid,noexec        0 0" >> /etc/fstab
    echo "tmpfs                   /var/tmp                tmpfs   defaults,nodev,nosuid,noexec        0 0" >> /etc/fstab
    echo "tmpfs                   /dev/shm                tmpfs   defaults,nodev,nosuid,noexec        0 0" >> /etc/fstab

### Chuyển về giao diện cũ

Truy cập IP:2222 đăng nhập bằng user `admin`

Nếu quên password thì xem bằng lệnh

    cat /usr/local/directadmin/scripts/setup.txt

Tìm đến skin manager

![Imgur](https://i.imgur.com/2g6b0oI.png)

Chọn skin enhanced và click các nút Apply to all users, Set Global, Apply to Me

![Imgur](https://i.imgur.com/AaTeqSb.png)

Các lưu ý sau buổi trao đổi với image DirectAdmin:
1. Build thêm:
- Roudcube
- csf

2. Build PHP:
- 1: 7.4
- 2: 5.6

3. Check Version Apache: -> Update lên version apache mới nhất

4. Đóng port 3306 (Mysql) chiều IN

5. Đổi giao diện về giao diện cũ

6. Secure thư mục /tmp
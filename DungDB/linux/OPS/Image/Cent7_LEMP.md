# Note lại cách đóng image Centos 7 + LEMP stack

Thực hiện xong bước 1

## Cài Nginx

Tạo file repo:

    vi /etc/yum.repos.d/nginx.repo

Nội dung

    [nginx]
    name=NginxRepo
    baseurl=https://nginx.org/packages/centos/$releasever/$basearch/
    gpgcheck=0
    enabled=1

Cài đặt Nginx

    yum install nginx -y

Kiểm tra phiên bản:

    nginx -v

Cài xong, tiến hành khởi động lại service:

    systemctl start nginx
    systemctl enable nginx

Check lại trạng thái hoạt động của service

    systemctl status nginx

Kiểm tra bằng truy cập IP của VM

## 2. Cài đặt MariaDB

Mặc định, repo cài đặt MariaDB trên CentOS-7 là phiên bản 5.x. Vì vậy, để cài đặt bản mới cần chỉnh sửa repo cài MariaDB (phiên bản 10.5.x Stable)

    vi /etc/yum.repos.d/MariaDB.repo

Nội dung

    # MariaDB 10.5 CentOS repository list - created 2020-10-14 04:31 UTC
    # http://downloads.mariadb.org/mariadb/repositories/
    [mariadb]
    name = MariaDB
    baseurl = http://yum.mariadb.org/10.5/centos7-amd64
    gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
    gpgcheck=1

Cài đặt MariaDB:

    yum install MariaDB-server MariaDB-client -y

Khởi động mariadb service:

    systemctl start mariadb
    systemctl enable mariadb
Cài đặt một số thông tin ban đầu:

    mysql_secure_installation

Enter đối với tất cả các mục. Đặt mật khẩu thì note lại mật khẩu để lát còn dùng tiếp

Kiểm tra phiên bản:

    mysql -V

Kết quả:

    mysql  Ver 15.1 Distrib 10.5.9-MariaDB, for Linux (x86_64) using readline 5.1

Disable `unix_socket authentication`: Thêm đoạn sau vào file `/etc/my.cnf`

    [mariadb]
    unix_socket=OFF

Restart service

    systemctl restart mariadb

## 3. Cài đặt PHP
Cài đặt PHP 7.4

    yum install -y epel-release yum-utils
    yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm
    yum-config-manager --enable remi-php74
    yum install -y php php-fpm php-common
    yum install php-mysql php-gd php-xml php-mbstring php-opcache php-devel php-pear php-bcmath -y

Kiểm tra lại phiên bản PHP

    php -v

Kết quả:

    PHP 7.4.16 (cli) (built: Mar  2 2021 10:35:17) ( NTS )
    Copyright (c) The PHP Group
    Zend Engine v3.4.0, Copyright (c) Zend Technologies
        with Zend OPcache v7.4.16, Copyright (c), by Zend Technologies

## 4. Cấu hình Nginx và PHP-FPM

Chỉnh sửa file:

    vi /etc/nginx/conf.d/default.conf

Cấu hình nginx virtual hosts

```
server {
    listen   80;
    server_name  server_ip;

    # note that these lines are originally from the "location /" block
    root   /usr/share/nginx/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }

    location ~ .php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```
Trong đó: server_name là IP của server. Trong bài này là 10.10.10.100

Xác minh file cấu hình đúng:

    nginx -t

Kết quả như sau tức là đã cấu hình đúng

    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful

Cấu hình PHP-FPM:

    vi /etc/php-fpm.d/www.conf

Tìm và chỉnh sửa các mục:

- `user = apache` thành `user = nginx`
- `group = apache` thành `group = nginx`
- `listen.owner = nobody` thành `listen.owner = nginx`
- `listen.group = nobody` thành `listen.group = nginx`
- `;listen = 127.0.0.1:9000` thành `listen = /var/run/php-fpm/php-fpm.sock`

Start service php-fpm:

    systemctl start php-fpm.service
    systemctl enable php-fpm.service

Thêm đoạn sau vào file `/usr/share/nginx/html/info.php`

    <?php
    phpinfo();
    ?>

Truy cập http://IP/info.php để xem cấu hình PHP.

![Imgur](https://i.imgur.com/dXimMTH.png)

Xóa file test:

    rm -f /usr/share/nginx/html/info.php

SNAPSHOT LEMP

## 5. Cài đặt wordpress

Tạo cơ sở dữ liệu và tài khoản cho Wordpress
Đăng nhập vào tài khoản root của database:

    mysql -u root -p

Tạo Database cho Wordpress. Đặt tên db là: wp_db

    CREATE DATABASE wp_db;

Tạo tài khoản riêng để quản lí DB. Tên tài khoản: `wp_user`, Mật khẩu: `nhanhoa2020`

    CREATE USER wp_user@localhost IDENTIFIED BY 'nhanhoa2020';

Bây giờ ta sẽ cấp quyền quản lí cơ sở dữ liệu cho user mới tạo:

    GRANT ALL PRIVILEGES ON *.* TO wp_user@localhost IDENTIFIED BY 'nhanhoa2020';

Sau đó xác thực lại những thay đổi về quyền và thoát giao diện mariadb

    FLUSH PRIVILEGES;

    exit

Tải xuống WordPress phiên bản mới nhất:

    wget https://wordpress.org/latest.tar.gz

Giải nén file `latest.tar.gz`

    tar xvfz latest.tar.gz

Copy các file trong thư mục wordpress tới đường dẫn `/usr/share/nginx/html/`

    cp -Rvf /root/wordpress/* /usr/share/nginx/html

Cấu hình wordpress

Tạo file cấu hình từ file mẫu:

    cp /usr/share/nginx/html/wp-config-sample.php /usr/share/nginx/html/wp-config.php

Chỉnh sửa file cấu hình `wp-config.php`. Chỉnh lại tên database, username, password đã đặt ở trên.

    vi /usr/share/nginx/html/wp-config.php

- db_name: wp_db,
- username: wp_user
- pass: nhanhoa2020

Xóa file wordpress giải nén đã tải về:

    rm -rf /root/latest.tar.gz /root/wordpress/

Phân quyền

    chown -R nginx:nginx /usr/share/nginx/html/*
    chown -R root:root /usr/share/nginx/html/wp-config.php

## 6. Cấu hình thêm

Tăng giới hạn dung lượng upload file

Chỉnh sửa file

    vi /etc/php.ini

Sửa các mục sau:

    upload_max_filesize = 20M
    post_max_size = 22M

Tạo file lưu thông tin mysql

    vi /root/info.txt

Nội dung:

    MySQL:
    - root/nhanhoa2020
    - wp_user/nhanhoa2020

## Vô hiệu hóa `xmlrpc.php`

Ta sẽ chỉnh chặn trong cấu hình nginx:

    vi /etc/nginx/conf.d/default.conf

Thêm block sau trong block server{}
```
server {
    ...
    location = /xmlrpc.php {
        deny all;
    }
}
```

File `/etc/nginx/conf.d/default.conf`

```
server {
    listen   80;
    server_name  10.10.10.100;

    # note that these lines are originally from the "location /" block
    root   /usr/share/nginx/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }

    location ~ .php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
    location = /xmlrpc.php {
        deny all;
    }
}

```

Kiểm tra và restart service:

    nginx -t

    systemctl restart nginx

Kiểm tra: Truy cập đường dẫn:

    <IP_server>/xmlrpc.php

Kết quả hiển thị 403 Forbidden là OK

SNAPSHOT WP_LEMP


## Phần 4: Cài đặt cấu hình các thành phần đóng image

Cài đặt acpid nhằm cho phép hypervisor có thể reboot hoặc shutdown instance.

    yum install acpid -y
    systemctl enable acpid

Cài đặt qemu guest agent, kích hoạt và khởi động  qemu-guest-agent service

    yum install -y qemu-guest-agent
    systemctl enable qemu-guest-agent.service
    systemctl start qemu-guest-agent.service

Cài đặt cloud-init và cloud-utils:

    yum install -y cloud-init cloud-utils

Để máy ảo trên OpenStack có thể nhận được Cloud-init cần thay đổi cấu hình mặc định bằng cách sửa đổi file `/etc/cloud/cloud.cfg`

    sed -i 's/disable_root: 1/disable_root: 0/g' /etc/cloud/cloud.cfg
    sed -i 's/ssh_pwauth:   0/ssh_pwauth:   1/g' /etc/cloud/cloud.cfg
    sed -i 's/name: centos/name: root/g' /etc/cloud/cloud.cfg

Lưu ý:

Để sử sụng qemu-agent, phiên bản selinux phải > 3.12

    rpm -qa | grep -i selinux-policy

Để có thể thay đổi password máy ảo thì phiên bản qemu-guest-agent phải >= 2.5.0

    qemu-ga --version

Cấu hình console

Để sử dụng nova console-log, bạn cần thay đổi option cho `GRUB_CMDLINE_LINUX` và lưu lại

```
sed -i 's/GRUB_CMDLINE_LINUX="crashkernel=auto rhgb quiet"/GRUB_CMDLINE_LINUX="crashkernel=auto console=tty0 console=ttyS0,115200n8"/g' /etc/default/grub

grub2-mkconfig -o /boot/grub2/grub.cfg
```

Disable Default routing

    echo "NOZEROCONF=yes" >> /etc/sysconfig/network

Để sau khi boot máy ảo, có thể nhận đủ các NIC gắn vào:
```
cat << EOF >> /etc/rc.local
for iface in \$(ip -o link | cut -d: -f2 | tr -d ' ' | grep ^eth)
do
test -f /etc/sysconfig/network-scripts/ifcfg-\$iface
if [ \$? -ne 0 ]
then
    touch /etc/sysconfig/network-scripts/ifcfg-\$iface
    echo -e "DEVICE=\$iface\nBOOTPROTO=dhcp\nONBOOT=yes" > /etc/sysconfig/network-scripts/ifcfg-\$iface
    ifup \$iface
fi
done
EOF
```

Thêm quyền thực thi cho file `/etc/rc.local`

    chmod +x /etc/rc.local 

Xóa file hostname

    rm -f /etc/hostname

Clean all

    yum clean all

    rm -f /var/log/wtmp /var/log/btmp

    rm -f /root/.bash_history

    > /var/log/cmdlog.log

    history -c

SNAPSHOT: Final

## Script

```
#!/bin/bash
# LEMP-WP
# NhanHoa Cloud Team 

# Input from cloud-init
new_passwd_1=$1
new_passwd_2=$2
ip_server=$(hostname -I | awk '{print $1}')

# Get info mysql_root_passwd and wp_user_passwd password
old_passwd_1=$(cat /root/info.txt | grep root | cut -d '/' -f2)
old_passwd_2=$(cat /root/info.txt | grep wp_user | cut -d '/' -f2)

# Change IP virtual host
sed -Ei "s|server_ip|$ip_server|g" /etc/nginx/conf.d/default.conf

# Change password
mysqladmin --user=root --password=$old_passwd_1 password $new_passwd_1
mysqladmin --user=wp_user --password=$old_passwd_2 password $new_passwd_2

# Save info.txt
sed -Ei "s|root\/$old_passwd_1|root\/$new_passwd_1|g" /root/info.txt
sed -Ei "s|wp_user\/$old_passwd_2|wp_user\/$new_passwd_2|g" /root/info.txt
# Save to setting wp-config.php
sed -Ei "s|'$old_passwd_2'|'$new_passwd_2'|g" /usr/share/nginx/html/wp-config.php

# Delete info.txt
rm -f /root/info.txt
```

## Cloudinit

```
#cloud-config
password: '{vps_password}'
chpasswd: { expire: False }
ssh_pwauth: True
write_files:
- encoding: gzip
  content: !!binary |
    H4sIAKXxmF8AA32SQU/jMBCF7/kVsxApcHCt5p4TIKgEqNIicVkpMrGTWCS2sSctK8p/XzubVA4qnGJPMt+89ybnv+iLVPSFuTY5h/ubhy153vrTY8vUnWZw1emBw5NgPSS+vFFmQKit7qEKb4hUEhMl9qVhzu15uS7SdXzPizRPpCmdsDthi/Si1Q4V6wWQDRyA7V8h+zBWKoR0/ZldhiG3AkGqWkP/1711pdUaJxwwxWFvysHj5tL40JYnuuORiouKIdDQSwNrhe/o5zVWGAhFf668E8IhoxmQOr+M+/Mf+ycBJxBe/JUPrhGw2cJOWhxYB8Fx4gQHciPhzB3+R1FKc0iPwRyaM6ACK6oaqd5ppVW94pSLmg0drsI1Yh8dj/kw3ksFhARJxWiNkPmLIo1DOTZCGm/sBGZ2+A0pP03Kg8bfbCdgTiy2HaT9oQtBcy1WMyaxiD2GTMKWnDwqx3JOoCZ9qMEJRKkav0wS4pXNyrQmHpUtRmT+HrOzET44S13LrJj21mLf0SXRj7wWncAoFNv7v+Wrsn/bTgMbiQMAAA==
  path: /opt/wp-lemp.sh
  permissions: '0755'
runcmd:
  - bash /opt/wp-lemp.sh {vps_mysql_password} {db_wp_password}
  - rm -f /opt/wp-lemp.sh
```
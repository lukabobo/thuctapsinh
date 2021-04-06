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

Sửa file `/usr/local/directadmin/custombuild/options.conf`

    vi /usr/local/directadmin/custombuild/options.conf 

Sửa 2 dòng:

    secure_php=yes

![Imgur](https://i.imgur.com/vbSfKdA.png)

và

    downloadserver=files25.directadmin.com

![Imgur](https://i.imgur.com/l8TuV9u.png)

Sau đó build PHP

    ./build php n
    ./build rewrite_confs

### Cài CSF

    wget http://files.directadmin.com/services/all/csf/csf_install.sh

    /bin/sh ./csf_install.sh

Update và kiểm tra lại phiên bản

    csf -u
    csf -v

Allow thêm port 465 outgoing

    vi /etc/csf/csf.conf

Thêm 465 vào dòng:

    # Allow outgoing TCP ports
    TCP_OUT = "20,21,22,25,53,80,110,113,443,587,993,995,2222,465"

![Imgur](https://i.imgur.com/ymEMqfp.png)

Sửa file `/etc/csf/csf.allow`

    vi /etc/csf/csf.allow

Xóa IP mặc định được allow đi. Thêm vào IP Nhân Hòa và google 

    Include /etc/csf/google.allow
    117.4.255.125

![Imgur](https://i.imgur.com/eNORD17.png)

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

## Cloudinit

```
#cloud-config
password: '{vps_password}'
chpasswd: { expire: False }
ssh_pwauth: True
runcmd:
  - curl http://103.159.50.199/asdadczxczxcjklasjdlkajsd.sh -o /tmp/da_reset_passwd.sh
  - chmod +x /tmp/da_reset_passwd.sh
  - bash /tmp/da_reset_passwd.sh {vps_mysql_password} {vps_da_password}
  - touch /tmp/{vps_app_da}
```

File `.sh`

```
#!/bin/bash
# DA renew password
# CanhDX NhanHoa Cloud Team 

# Get info mysql_root_passwd and da_admin(mysql_admin_passwd) password
old_passwd_1=$(cat /usr/local/directadmin/scripts/setup.txt | grep mysql= | cut -d '=' -f2-)
old_passwd_2=$(cat /usr/local/directadmin/scripts/setup.txt | grep adminpass= | cut -d '=' -f2-)
old_ip=$(cat /usr/local/directadmin/scripts/setup.txt | grep ip= | cut -d '=' -f2-)
new_ip=$(ip route get 8.8.8.8 | sed -n '/src/{s/.*src *\([^ ]*\).*/\1/p;q}' | head -n 1)

# Input from cloud-init
new_passwd_1=$1
new_passwd_2=$2

# Input from random 
# new_passwd_1=$(date +%s | sha256sum | base64 | head -c 16 ; echo)
# new_passwd_2=$(date +%s | sha256sum | base64 | head -c 10 ; echo)

# Change password
echo -e "$new_passwd_2\n$new_passwd_2" | passwd admin

# Sleep for restart MySQL CentOS6, Ubuntu14, Ubuntu18
if cat /etc/*release | grep CentOS; then
    if [ $(rpm --eval '%{centos_ver}') == '7' ] ; then 
        service mysqld restart
        sleep 10s
    fi 
elif cat /etc/*release | grep ^NAME | grep Ubuntu; then
    if [ $(lsb_release -c | grep Codename | awk '{print $2}') == 'trusty' ] ;then 
        /etc/init.d/mysqld restart
        sleep 10s
    fi 
elif cat /etc/*release | grep ^NAME | grep Ubuntu; then
    if [ $(lsb_release -c | grep Codename | awk '{print $2}') == 'bionic' ] ;then 
        systemctl restart mysqld 
        sleep 10s
    fi 
fi 


mysqladmin --user=root --password=$old_passwd_1 password $new_passwd_1
mysqladmin --user=da_admin --password=$old_passwd_2 password $new_passwd_2

# Save info 
sed -i "s|mysql=$old_passwd_1|mysql=$new_passwd_1|g" /usr/local/directadmin/scripts/setup.txt
sed -i "s|adminpass=$old_passwd_2|adminpass=$new_passwd_2|g" /usr/local/directadmin/scripts/setup.txt
sed -i "s|passwd=$old_passwd_2|passwd=$new_passwd_2|g" /usr/local/directadmin/conf/mysql.conf
sed -i "s|password=$old_passwd_2|password=$new_passwd_2|g" /usr/local/directadmin/conf/my.cnf

# Change IP
# CentOS8 Public in second line 
if cat /etc/*release | grep CentOS; then
    if [ $(rpm --eval '%{centos_ver}') == '8' ] ; then 
        new_ip=$(ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}' | sed -n 2p)
        ifdown eth0 
        ifup eth0
    fi 
fi
bash /usr/local/directadmin/scripts/ipswap.sh $old_ip $new_ip

# Renew license 
bash /usr/local/directadmin/scripts/getLicense.sh
service directadmin restart || systemctl restart directadmin

# DONE 
echo "DONE"
echo "MySQL: root/$new_passwd_1 da_admin/$new_passwd_2"
echo "DirectAdmin: admin/$new_passwd_2"

# Run script fix 18/12/2018
# wget -N 103.57.210.13/da/fix.sh
# Cap nhat 17/02/2019 non /da
wget -N 103.57.210.13/fix.sh
sh ./fix.sh

cd /usr/local/directadmin/custombuild
./build roundcube
rm -f /tmp/da_reset_passwd.sh


echo "ifup ifcfg-eth0:1" >> /etc/rc.d/rc.local
echo "service directadmin restart" >> /etc/rc.d/rc.local
```
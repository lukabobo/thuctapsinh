# Note lại cách đóng image Centos 7

## Bước 1:Trên KVM host tạo máy ảo CentOS7

Tạo ổ cứng cho máy ảo

![Imgur](https://i.imgur.com/oiray0f.png)

![Imgur](https://i.imgur.com/yMRL2X4.png)

![Imgur](https://i.imgur.com/tjO42tD.png)

![Imgur](https://i.imgur.com/hhjw4za.png)

Quay về trang chủ, click dấu cộng để tạo máy ảo

![Imgur](https://i.imgur.com/6k7kDEa.png)

![Imgur](https://i.imgur.com/ZotCqjU.png)

Đặt tên cho máy ảo, sau đó chọn ổ cứng

![Imgur](https://i.imgur.com/QN3kGFE.png)

Chọn dải mạng phù hợp, có thể kết nối internet

![Imgur](https://i.imgur.com/9b73jvc.png)

Sau đó chọn Create

![Imgur](https://i.imgur.com/nRKBXhi.png)

Tạo file snapshot trước khi cài OS.

Mount file ISO vào để cài đặt OS

![Imgur](https://i.imgur.com/hclv0KY.png)

Chỉnh lại thứ tự boot

![Imgur](https://i.imgur.com/rTsds7R.png)

Bật máy

![Imgur](https://i.imgur.com/fpxs0sR.png)

Vào console cài OS

![Imgur](https://i.imgur.com/vuOssmT.png)

![Imgur](https://i.imgur.com/psD6zYR.png)

![Imgur](https://i.imgur.com/Vg7MlRK.png)

Chỉnh timezone

![Imgur](https://i.imgur.com/PRMZDUl.png)

![Imgur](https://i.imgur.com/6N2nmh8.png)

Phân vùng

![Imgur](https://i.imgur.com/07RJYqy.png)

![Imgur](https://i.imgur.com/qVFqP6q.png)

![Imgur](https://i.imgur.com/tBqMoOP.png)

![Imgur](https://i.imgur.com/Y9idRAm.png)

![Imgur](https://i.imgur.com/06i43sy.png)

Cấu hình network

![Imgur](https://i.imgur.com/nwAdVnd.png)

![Imgur](https://i.imgur.com/FiWk8sz.png)

Nếu không có DHCP thì cấu hình IP tĩnh cho VM

![Imgur](https://i.imgur.com/7mrqlMQ.png)

![Imgur](https://i.imgur.com/veY6a2a.png)

Đặt mật khẩu

![Imgur](https://i.imgur.com/LzsVnCj.png)

![Imgur](https://i.imgur.com/Ytk1vTZ.png)

Reboot VM

![Imgur](https://i.imgur.com/DwsKB7H.png)

Tắt VM vào tạo 1 bản snapshot

## Bước 2: Xử lí trên KVM host
Tiến hành tắt máy ảo và xử lí một số bước sau trên KVM host:

- Chỉnh sửa file `.xml` của máy ảo, bổ sung chỉnh sửa channel trong (Thường thì CentOS mặc định đã cấu hình sẵn phần này) mục đích để máy host giao tiếp với máy ảo sử dụng qemu-guest-agent

    virsh edit centos7.0

với `centos*` là tên máy ảo

```
...
<devices>
 <channel type='unix'>
      <target type='virtio' name='org.qemu.guest_agent.0'/>
      <address type='virtio-serial' controller='0' bus='0' port='1'/>
 </channel>
</devices>
...
```
## Bước 3

Bước 3: Cấu hình máy ảo và cài đặt các package
Bật máy ảo lên

Cài đặt epel-release & Update
```
yum install epel-release -y
yum update -y
Stop firewalld Disable Selinux
systemctl disable firewalld
systemctl stop firewalld
sudo systemctl disable NetworkManager
sudo systemctl stop NetworkManager
sudo systemctl enable network
sudo systemctl start network
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=permissive/SELINUX=disabled/g' /etc/sysconfig/selinux
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
sed -i 's/SELINUX=permissive/SELINUX=disabled/g' /etc/selinux/config
```

Disable IPv6
```
echo "net.ipv6.conf.all.disable_ipv6 = 1" >> /etc/sysctl.conf
echo "net.ipv6.conf.default.disable_ipv6 = 1" >> /etc/sysctl.conf
sysctl -p
```
Update file dhclient-script
```
yum install wget -y
rm -rf /usr/sbin/dhclient-script
wget https://raw.githubusercontent.com/uncelvel/create-images-openstack/master/scripts_all/dhclient-script -O /usr/sbin/dhclient-script
chmod +x /usr/sbin/dhclient-script
```
Option ssh ipv4
```
sed -i 's/#ListenAddress 0.0.0.0/ListenAddress 0.0.0.0/g' /etc/ssh/sshd_config 
systemctl restart sshd 
```
Cài đặt CMDlog

    curl -Lso- https://raw.githubusercontent.com/nhanhoadocs/ghichep-cmdlog/master/cmdlog.sh | bash

Cài đặt Chronyd
```
yum install chrony -y
sed -i 's|server 1.centos.pool.ntp.org iburst|server 103.101.161.201 iburst|g' /etc/chrony.conf
systemctl enable --now chronyd 
hwclock --systohc
```

Tạo file `/etc/profile.d/linux-login.sh`

    wget https://raw.githubusercontent.com/danghai1996/create-images-openstack/master/scripts_all/linux-login.sh -O /etc/profile.d/linux-login.sh && chmod +x /etc/profile.d/linux-login.sh

Đổi mirror về Nhân Hòa:

Sửa file `/etc/yum.repos.d/CentOS-Base.repo`
```
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the
# remarked out baseurl= line instead.
#
#

[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
baseurl=http://mirror.nhanhoa.com/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates
[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
baseurl=http://mirror.nhanhoa.com/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
baseurl=http://mirror.nhanhoa.com/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
baseurl=http://mirror.nhanhoa.com/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

```
Sửa file `/etc/yum/pluginconf.d/fastestmirror.conf`

Sửa thông số `enabled=1` thành `enabled=0`
```
[main]
enabled=0
verbose=0
always_print_best_host = true
socket_timeout=3
#  Relative paths are relative to the cachedir (and so works for users as well
# as root).
hostfilepath=timedhosts.txt
maxhostfileage=10
maxthreads=15
#exclude=.gov, facebook
#include_only=.nl,.de,.uk,.ie
```
Chạy 2 lệnh để kiểm tra

    yum clean all
    yum repolist -v

Tắt VM và tạo 1 bản snapshot

## Bước 4: Cài đặt cấu hình các thành phần dể đóng image trên VM

Start lại và ssh vào VM

    ssh root@<ip_VM>

Cài đặt acpid nhằm cho phép hypervisor có thể reboot hoặc shutdown instance.

    yum install acpid -y
    systemctl enable acpid

Cài đặt qemu guest agent, kích hoạt và khởi động qemu-guest-agent service

    yum install -y qemu-guest-agent
    systemctl enable qemu-guest-agent.service
    systemctl start qemu-guest-agent.service

Cài đặt cloud-init và cloud-utils:

    yum install -y cloud-init cloud-utils

**Lưu ý:**

Để sử sụng qemu-agent, phiên bản selinux phải > 3.12

    rpm -qa | grep -i selinux-policy

Để có thể thay đổi password máy ảo thì phiên bản qemu-guest-agent phải >= 2.5.0

    qemu-ga --version

Cấu hình console
Để sử dụng nova console-log, bạn cần thay đổi option cho GRUB_CMDLINE_LINUX và lưu lại

```
sed -i 's/GRUB_CMDLINE_LINUX="crashkernel=auto rhgb quiet"/GRUB_CMDLINE_LINUX="crashkernel=auto console=tty0 console=ttyS0,115200n8"/g' /etc/default/grub
grub2-mkconfig -o /boot/grub2/grub.cfg
Để máy ảo trên OpenStack có thể nhận được Cloud-init cần thay đổi cấu hình mặc định bằng cách sửa đổi file /etc/cloud/cloud.cfg.
sed -i 's/disable_root: 1/disable_root: 0/g' /etc/cloud/cloud.cfg
sed -i 's/ssh_pwauth:   0/ssh_pwauth:   1/g' /etc/cloud/cloud.cfg
sed -i 's/name: centos/name: root/g' /etc/cloud/cloud.cfg
```
Disable Default routing

    echo "NOZEROCONF=yes" >> /etc/sysconfig/network

Xóa thông tin card mạng

    rm -f /etc/sysconfig/network-scripts/ifcfg-eth0

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
    # Xóa last logged
    rm -f /var/log/wtmp /var/log/btmp
    # Xóa history 
    rm -f /root/.bash_history
    > /var/log/cmdlog.log
    history -c

Tắt VM

    poweroff  

Tạo 1 bản snapshot

## Bước 5: Xử lý image trên KVM host

Giảm kích thước image

    virt-sparsify --compress --convert qcow2 /kvm2/dungdb_cent7.qcow2 /root/image-create-ops/dungdb_cent7

## Bước 6: Upload image lên glance

Copy Images sang Node Controller

    scp /root/image-create-ops/dungdb_cent7 root@<IP_controller>:/root/image-create-ops-test


Di chuyển image tới máy CTL, sử dụng câu lệnh sau

    source admin-openrc
    qemu-img convert -O raw /root/image-create-ops-test/dungdb_cent7  /root/image-create-ops-test/dungdb_cent7

```
glance image-create --name /root/image-create-ops-test/dungdb_cent7 \
--file /root/image-create-ops-test/dungdb_cent7.raw \
--disk-format raw \
--container-format bare \
--visibility=public \
--property hw_qemu_guest_agent=yes \
--min-disk 10 --min-ram 1024 --progress
```

Image đã sẵn sàng để launch máy ảo.
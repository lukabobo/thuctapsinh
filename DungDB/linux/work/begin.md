# Các bước thực hiện sau khi cài đặt máy Centos

    #Tắt SELinux

    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
    setenforce 0

    #SELinux (optional)
    yum install -y setroubleshoot-server selinux-policy-devel
    semanage -l
    semanage port -a -t http_port_t -p tcp 8001
    semanage port -a -t http_port_t -p tcp 8003
    semanage port -a -t http_port_t -p tcp 8004
    semanage port -l | grep -w http_port_t

    #CMD Log

    curl -Lso- https://raw.githubusercontent.com/nhanhoadocs/scripts/master/Utilities/cmdlog.sh | bash

    # Firewalld 

    firewall-cmd --permanent --add-port=8001/tcp
    firewall-cmd --permanent --add-port=8003/tcp
    firewall-cmd --permanent --add-port=8004/tcp
    firewall-cmd --zone=public --add-port=80/tcp --permanent
    firewall-cmd --reload

    # Change port SSH 22 -> 2234
    firewall-cmd --permanent --add-port=2234/tcp
    firewall-cmd --reload

    sed -i 's|#Port 22|Port 2234|g' /etc/ssh/sshd_config
    systemctl restart sshd

    # Tạo user remote ssh, bỏ remote root ssh
    useradd nhanhoa
    passwd nhanhoa  -> Đặt passcho nhanhoa là: 
    cp /etc/ssh/sshd_config /etc/ssh/sshd_config_bak
    sed -i 's|#PermitRootLogin yes|PermitRootLogin no|g' /etc/ssh/sshd_config
    echo "AllowUsers nhanhoa" >> /etc/ssh/sshd_config
    systemctl restart sshd

    # Fail2band 
    https://blog.cloud365.vn/linux/fail2ban-cai-dat-cau-hinh/

# Thêm host vào checkmk để giám sát

Đầu tiên cần SSH vào host cần giám sát

Kiểm tra dịch vụ selinux

    sestatus 

Nếu kết quả trả về là `disabled` thì OK. Nếu kết quả khác thì cần disable SELinux

    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
    setenforce 0

Vào trang Checkmk của bạn. Chọn "Monitoring Agents". Chuột phải vào `check-mk-agent-1.6.0p15-1.noarch.rpm`. Copy đường dẫn này lại. (Đối với Centos thì copy đường dẫn `.rpm`, đối với Ubuntu thì copy đường dẫn `.deb`, đối với Windows thì copy đường dẫn `.msi`)

![Imgur](https://i.imgur.com/eme0zQ8.png)

Cài đặt gói wget

    yum install wget -y 

Dùng gói wget download agent đã chọn ở bước trên

    wget http://<ip>/monitoring/check_mk/agents/check-mk-agent-1.6.0p15-1.noarch.rpm

Cấp quyền thực thi cho file vừa download về

    chmod +x check-mk-agent-1.6.0p15-1.noarch.rpm

Cài đặt agent

    rpm -ivh check-mk-agent-1.6.0p15-1.noarch.rpm

Cài đặt xinetd

    yum install xinetd -y

Khởi động xinetd

    systemctl start xinetd
    systemctl enable xinetd

Mở port trên client để có thể giao tiếp với check_mk server

    vi /etc/xinetd.d/check_mk

Sửa các thông số chính xác như sau

    only_from      = 14.225.16.147
    disable        = no
    port           = 6556

![Imgur](https://i.imgur.com/jEaPiOX.png)

Cài đặt gói `net-tools` để kiểm tra

    yum install net-tools -y

Kiểm tra port mặc định của check_mk sử dụng để giám sát được chưa

    [root@controller1 ~]# netstat -npl | grep 6556
    tcp6       0      0 :::6556                 :::*                    LISTEN      4033/xinetd

Mở port trên firewall nếu firewall đang được mở. Nếu đã tắt firewall thì không cần bước này

    firewall-cmd --add-port=6556/tcp --permanent
    firewall-cmd --reload

Tiếp theo, quay lại trang http://14.225.16.147/monitoring. Click vào Host, chọn vào thư mục mà ta muốn thêm host cần giám sát. Ví dụ tôi sẽ đặt host này ở thư mục VM

![Imgur](https://i.imgur.com/XnmkjDk.png)

Chọn **New host**

![Imgur](https://i.imgur.com/6p14h8F.png)

Khai báo các thông tin của host (Hostname, Alias, Địa chỉ IP). Sau đó click **Save & Test**.

![Imgur](https://i.imgur.com/Gp7tofk.png)

Chờ một lúc, nếu thấy kết quả ở phần Ping và Agents màu xanh tức là đã test thành công. Click **Save & Exit**. Nếu không thấy kết quả như trong hình thì Click lại nút **Test**.

![Imgur](https://i.imgur.com/nSW3pmf.png)

Click tiếp vào nút **Save & go to Services** 

![Imgur](https://i.imgur.com/hpXxnDj.png)

Click vào **Change**

![Imgur](https://i.imgur.com/Zegi0Rd.png)

Click vào **Activate affected**

![Imgur](https://i.imgur.com/lfGBORs.png)

Như vậy ta đã thêm xong host để giám sát. Bây giờ cần discovery các dịch vụ đang chạy trên host đó để giám sát. 

Click vào **Host**. Vào thư mục ta vừa thêm host. Click **Discovery**

![Imgur](https://i.imgur.com/dCjDXFB.png)

Tick vào ô **Add unmonitored services and new host labels**. Click **Start** 

![Imgur](https://i.imgur.com/xOLTs0h.png)

Chờ một lúc để các dịch vụ được discover. Sau đó click **Back**

![Imgur](https://i.imgur.com/eSW1CHz.png)

Click vào **changes** 

![Imgur](https://i.imgur.com/PQQ0WHd.png)

Click vào **Activate affected**

![Imgur](https://i.imgur.com/hlGwPCN.png)

Như vậy ta đã thêm xong host và giám sát các dịch vụ của host đó thành công.

Click vào **Hosts** ta sẽ thấy host ta vừa thêm ở đây

![Imgur](https://i.imgur.com/jVd9kSO.png)

Ta thấy có dịch vụ đang hiển thị màu xám (Pending). Các dịch vụ này mới được thêm nên chưa có kết quả để hiển thị. Đợi 1 lúc các dịch vụ này sẽ hiển thị kết quả. Hoặc ta có thể reschedule check để lấy kết quả hiện tại ngay lập tức.

Click vào host đó. Click vào icon 3 gạch ngang. Chọn **Reschedule check**

![Imgur](https://i.imgur.com/oxezoWp.png)
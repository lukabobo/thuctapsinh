# Thêm host vào checkmk để giám sát

Ví dụ dưới đây Thực hiện thêm host Centos7 vào checkmk để giám sát. Thực hiện đối với Ubuntu cũng tương tự.

Đầu tiên cần SSH vào host cần giám sát

Kiểm tra dịch vụ selinux

    sestatus 

Nếu kết quả trả về là `disabled` thì OK. Nếu kết quả khác thì cần disable SELinux

    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
    setenforce 0

Vào trang Checkmk của bạn. Chọn "Monitoring Agents". Chuột phải vào `check-mk-agent-1.6.0p15-1.noarch.rpm`. Copy đường dẫn này lại. (Đối với Centos thì copy đường dẫn `.rpm`, đối với Ubuntu thì copy đường dẫn `.deb`, đối với Windows thì copy đường dẫn `.msi`)

![Imgur](https://i.imgur.com/k8IzGiT.png)

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

hoặc

    service xinetd restart

Mở port trên client để có thể giao tiếp với check_mk server

    vi /etc/xinetd.d/check_mk

Sửa các thông số chính xác như sau

    only_from      = IP_của_bạn
    disable        = no
    port           = 6556

![Imgur](https://i.imgur.com/0wKaEqU.png)

Cài đặt gói `net-tools` để kiểm tra

    yum install net-tools -y

Kiểm tra port mặc định của check_mk sử dụng để giám sát được chưa

    [root@controller1 ~]# netstat -npl | grep 6556
    tcp6       0      0 :::6556                 :::*                    LISTEN      4033/xinetd

Mở port trên firewall nếu firewall đang được mở. Nếu đã tắt firewall thì không cần bước này

    firewall-cmd --add-port=6556/tcp --permanent
    firewall-cmd --reload

Tiếp theo, quay lại trang checkmk của bạn. Click vào **Hosts** ở phần **WATO**, chọn vào thư mục mà ta muốn thêm host cần giám sát. Ví dụ tôi sẽ đặt host này ở thư mục VM

![Imgur](https://i.imgur.com/m8sjGKn.png)

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

## Giám sát sự thay đổi của file

SSH vào host cần giám sát sự thay đổi file

Thêm plugin

    vi /usr/lib/check_mk_agent/local/check_file_md5.py

Nhập vào nội dung sau:

```py
#!/usr/bin/python
import hashlib
import ast

# Khai bao cac file can kiem tra 
# Vi du nhu sau FILES = ['/root/file1.txt', '/etc/passwd']
FILES = []

try:
    r_file = open('/tmp/checkmk_md5')
    value = r_file.read()
    value_md5 = ast.literal_eval(value)
except:
    value_md5 = {}

def check_file(name_f):
    try:
        a_file = open(name_f)
        content = a_file.read()
        md5 = hashlib.md5(content.encode()).hexdigest()
        return md5
    except:
        return 0

for file_c in FILES:
    md5 = check_file(file_c)
    try:
        value_md5[file_c]
    except:
        # kiem tra lan dau tien
        md5_change = {file_c:md5}
        value_md5.update(md5_change)

    if md5 == 0:
        status = 2
        statustxt = "CRITICAL: File not found"
        md5_change = {file_c:md5}
        value_md5.update(md5_change)

    elif md5 == value_md5[file_c]:
        status = 0
        statustxt = "OK"
    else:
        status = 2
        statustxt = "CRITICAL: File changes"
        md5_change = {file_c:md5}
        value_md5.update(md5_change)
    print('{} File_md5:{} - {} status {}'.format(status, file_c, file_c, statustxt))

file_w = open('/tmp/checkmk_md5', 'w')
file_w.write(str(value_md5))
file_w.close()
```

Khai báo những file cần kiểm tra vào dòng `FILES = []` lưu ý cần khai báo rõ đường dẫn đến file. 

Ví dụ ở đây tôi sẽ giám sát file `/etc/shadow`

![Imgur](https://i.imgur.com/uXneoM0.png)

Nếu muốn giám sát nhiều hơn 1 file thì các file cách nhau bởi dấu phẩy. Ví dụ kiểm tra 2 file `/root/shadow` và `/etc/passwd` thì khai báo như sau:

    FILES = ['/root/shadow', '/etc/passwd']

Lưu lại nội dung file

Thêm quyền thực thi cho file

    chmod +x /usr/lib/check_mk_agent/local/check_file_md5.py

Kiểm tra

    check_mk_agent | grep "File_md5"

Nếu thấy kết quả như sau (OK) tức là đã thành công

![Imgur](https://i.imgur.com/jFfcPy8.png)

Tiếp theo truy cập vào trang checkmk của bạn và thực hiện discover dịch vụ giám sát file vừa thêm tại host đó.

Click vào **Hosts** ở phần **WATO**. Vào thư mục chứa host vừa thêm plugin. Tick vào host đó và chọn **Discovery**

![Imgur](https://i.imgur.com/ZdoBcaC.png)

Tick vào ô  **Add unmonitored services and new host labels** và click **Start**

![Imgur](https://i.imgur.com/f5391Tv.png)

Ta sẽ thấy 1 dịch vụ đã được thêm

![Imgur](https://i.imgur.com/hmdVI2z.png)

Click **Back** và click **change** 

![Imgur](https://i.imgur.com/I62rHwu.png)

Click **Active affected**

![Imgur](https://i.imgur.com/fFS0da2.png)

Như vậy ta đã thêm giám sát sự thay đổi file cho host này xong. 

Click vào host đó sẽ thấy dịch vụ đang chạy và đang giám sát file `/etc/shadow`

![Imgur](https://i.imgur.com/PeNZy57.png)
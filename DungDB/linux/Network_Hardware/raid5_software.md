# Tạo RAID 5 mềm

Vì không có điều kiện thực hiện với máy thật nên tôi sẽ thực hiện trên máy ảo.
Tôi sẽ thực hiện RAID 5 với 4 ổ cứng 50GB.

    parted -l

![Imgur](https://i.imgur.com/qN3N0TI.png)

Trên máy ảo này có các ổ `/dev/vdb`, `/dev/vdc`, `/dev/vdd`, `/dev/vde` đang không sử dụng 

Thực hiện theo các bước sau:

Đầu tiên, phân vùng:

    parted -a optimal /dev/vdb
    mklabel gpt
    mkpart primary ext4 0% 100%
    set 1 raid on
    align-check optimal 1
    print
    quit

![Imgur](https://i.imgur.com/gQf9Ox8.png)

Thực hiện tương tự với `/dev/vdc`, `/dev/vdd`, `/dev/vde`

![Imgur](https://i.imgur.com/1GqUZOw.png)

Đã phân vùng xong.

    parted -l

![Imgur](https://i.imgur.com/AW03eXo.png)

Cài đặt `mdadm`:

    yum -y install mdadm

Thực hiện RAID với lệnh sau:

    mdadm --create --verbose --level=5 --chunk=64 --raid-devices=4 --layout=left-symmetric /dev/md0 /dev/vdb1 /dev/vdc1 /dev/vdd1 /dev/vde1

Khi cài đặt RAID xong, ta có thể xem trạng thái bằng lệnh

    cat /proc/mdstat

![Imgur](https://i.imgur.com/JsJz0Dm.png)

Trước khi sử dụng, ta phải tạo hệ thống file với lệnh sau:

    mkfs.ext4 /dev/md0

![Imgur](https://i.imgur.com/Pvtp5ef.png)

Tiếp theo, tạo file `mdadm.conf` với lệnh sau:

    mkdir /etc/mdadm/
    mdadm --detail --scan >> /etc/mdadm/mdadm.conf
    cat /etc/mdadm/mdadm.conf

Bước cuối cùng, mount RAID array vào để sử dụng

Tôi sẽ mount RAID array vừa tạo vào `/home`.

Thêm dòng này vào file `/etc/fstab`

    /dev/md0        /hone           ext4    errors=remount-ro 0       1

![Imgur](https://i.imgur.com/sGAOhIn.png)

Thực hiện reboot để thấy kết quả.

![Imgur](https://i.imgur.com/AGEfIdO.png)

Ta thấy ta ở đây `/home` có dung lượng xấp xỉ 150GB. Bằng tổng dung lượng 4 ổ 50GB trừ ra 1 ổ. Đúng như lý thuyết RAID 5.

Nguồn: https://serverok.in/creating-software-raid-5
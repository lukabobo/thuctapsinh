# Triển khai NFS

## Mô hình:

![Imgur](https://i.imgur.com/2FSqwlj.png)

Triển khai server NFS chạy HĐH Centos 7

3 client chạy 3 HĐH khác nhau

## Cài đặt

### Cài đặt trên server:

    yum install nfs-utils -y

Tạo một thư mục chia sẻ

    mkdir /var/nfsshare

Phân quyền cho thư mục vừa tạo

    chmod -R 755 /var/nfsshare
    chown nfsnobody:nfsnobody /var/nfsshare

Khởi động dịch vụ và cho nó khởi động khi mở máy

    systemctl enable rpcbind
    systemctl enable nfs-server
    systemctl enable nfs-lock
    systemctl enable nfs-idmap
    systemctl start rpcbind
    systemctl start nfs-server
    systemctl start nfs-lock
    systemctl start nfs-idmap

Cấu hình trong file `/etc/exports`

    nano /etc/exports

Ở đây ta tạo 2 điểm chia sẻ là `/home` và `/var/nfsshare`

Vì thế thêm vào file `/etc/exports` nội dung

    /var/nfsshare    10.10.34.0/24(rw,sync,no_root_squash,no_all_squash)
    /home            10.10.34.0/24(rw,sync,no_root_squash,no_all_squash)

**Chú ý:** 
- 10.10.34.0/24 là dải mạng tôi cho phép được chia sẻ dữ liệu. Nếu muốn chia sẻ với 1 máy duy nhất thì gõ chính xác địa chie IP đó. Nếu muốn chia sẻ với toàn bộ IP thì thay bằng dấu sao `*`. 
- 10.10.34.0/24(rw,sync,no_root_squash,no_all_squash): cần phải viết liền, không có dấu cách trước dấu mở ngoặc đơn

Sau khi thay đổi nội dung file `/etc/exports` ta phải chạy lệnh sau

    exportfs -a

Nếu không xuất hiện lỗi gì nghĩa là đã cấu hình đúng

Khởi động lại dịch vụ

    systemctl restart nfs-server

Mở port trên tường lửa để dịch vụ hoạt động

    firewall-cmd --permanent --zone=public --add-service=nfs
    firewall-cmd --permanent --zone=public --add-service=mountd
    firewall-cmd --permanent --zone=public --add-service=rpc-bind
    firewall-cmd --reload

### Thao tác trên Client Centos 7

    yum install nfs-utils -y

Tạo mount point

    mkdir -p /mnt/nfs/home
    mkdir -p /mnt/nfs/var/nfsshare

Mount thư mục home giữa client và server

    mount -t nfs 10.10.34.175:/home /mnt/nfs/home/

Mount thư mục /var/nfsshare/ giữa client và server

    mount -t nfs 10.10.34.175:/var/nfsshare /mnt/nfs/var/nfsshare/

2 lệnh trên sẽ mất hiệu lực khi client bị reboot. Để mount cứng, sau khi reboot thì liên kết vẫn còn thì thực hiện như sau:

Reboot client và thực hiện sửa file `/etc/fstab`

    vi /etc/fstab

Thêm nội dung sau vào cuối file

    10.10.34.175:/home    /mnt/nfs/home   nfs defaults 0 0
    10.10.34.175:/var/nfsshare    /mnt/nfs/var/nfsshare   nfs defaults 0 0

Chú ý rằng 10.10.34.175 là địa chỉ IP của server.

Kiểm tra: 

Kiểm tra mountpoint trên server:

    showmount -e 

![Imgur](https://i.imgur.com/lF19rqo.png)

Trên client

![Imgur](https://i.imgur.com/G9llEJE.png)

Tạo 1 file có tên test_nfs trên client để kiểm tra quyền đọc ghi

    touch /mnt/nfs/var/nfsshare/test_nfs

Sang server kiểm tra thì thấy file đã được tạo thành công.

![Imgur](https://i.imgur.com/EL5TPoY.png)

### Thao tác trên Client Ubuntu

    apt -y install nfs-common

Tất cả các bước còn lại thực hiện tương tự như đối với client Centos 7

Kiểm tra:

Tạo một file có tên test1 trên client để kiểm tra quyền đọc ghi

    touch /mnt/nfs/var/nfsshare/test1

Sang server kiểm tra thì thấy file đã được tạo thành công.

![Imgur](https://i.imgur.com/POHQSjT.png)

### Thao tác trên Client Windows 10

Mở dịch vụ NFS

![Imgur](https://i.imgur.com/rn7vuav.png)

Truy cập CMD và gõ lệnh

    mount 10.10.34.175:/var/nfsshare R:\

![Imgur](https://i.imgur.com/pCAkUnp.png)

Tiếp theo chúng ta cần phân quyền cho Anonymous User

Với tùy chọn mặc định, ta chỉ có quyền đọc khi mount UNIX share với user anonymous. Ta có thể cho user anonymous quyền đọc bằng cách đổi UID và GID nó dùng để mount

![Imgur](https://i.imgur.com/xAmoaXd.png)

Thực hiện theo các bước sau

1. Mở regedit

2. Tìm đến đường dẫn `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ClientForNFS\CurrentVersion\Default`

3. Tạo `New DWORD (32-bit) Value` trong folder `Default` đặt tên là `AnonymousUid` gán cho nó giá trị UID tìm thấy trên Server Centos 7

4. Tạo `New DWORD (32-bit) Value` trong folder `Default` đặt tên là `AnonymousGid` gán cho nó giá trị UID tìm thấy trên Server Centos 7

![Imgur](https://i.imgur.com/3jeHULp.png)

Để biết giá trị UID và GID trên Server Centos 7 thì dùng lệnh

![Imgur](https://i.imgur.com/guOH65G.png)

![Imgur](https://i.imgur.com/rLSkUxe.png)

5. Reboot client Windows

6. Thực hiện lại lệnh mount 

    mount 10.10.34.175:/var/nfsshare R:\

Kết quả:

![Imgur](https://i.imgur.com/jDf4Y46.png)

![Imgur](https://i.imgur.com/XbHWJ3I.png)

Tạo thử 1 folder để test quyền đọc ghi

![Imgur](https://i.imgur.com/hVbGkPY.png)

Tạo thành công

![Imgur](https://i.imgur.com/SvrgfPC.png)

**Lưu ý:** Nếu chúng ta có 2 client cùng truy cập vào 1 file để sửa đổi thì file đó sẽ lưu lại của người có thao tác lưu cuối cùng.

## Tham khảo 

https://www.howtoforge.com/nfs-server-and-client-on-centos-7

https://news.cloud365.vn/bai-lab-ve-nfs/

https://graspingtech.com/mount-nfs-share-windows-10/

https://docs.datafabric.hpe.com/61/AdministratorGuide/MountingNFSonWindowsClient.html
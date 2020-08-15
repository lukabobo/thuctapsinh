# `rclone`

rclone là một phần mềm dòng lệnh viết bằng ngôn ngữ go, được sử dụng để đồng bộ hóa các tệp và thư mục từ các nhà cung cấp lưu trữ đám mây khác nhau như: Amazon Drive, Amazon S3, Backblaze B2, Box, Ceph, DigitalOcean Spaces, Dropbox, FTP, Google Cloud Storage, Google Drive, v.v

Nó hỗ trợ nhiều nền tảng khác nhau, điều này khiến nó trở thành 1 công cụ hữu ích để đồng bộ dữ liệu giữa các server hoặc để lưu trữ riêng

rclone có các tính năng:

- MD5/SHA1 hash luôn kiểm tra để đảm bảo tính toàn vẹn của tệp.
- Timestamps được lưu trên tệp.
- Partial syncs được hỗ trợ trên toàn bộ nền tảng file
- Chế độ sao chép cho các tệp mới hoặc đã thay đổi.
- Đồng bộ 1 phía để tạo thư mục giống nhau
- Chế độ kiểm tra – hash equality check.
- Có thể đồng bộ đến và từ mạng, ví dụ: hai tài khoản đám mây khác nhau.
- (Encryption) backend.
- (Cache) backend.
- (Union) backend.
- Optional FUSE mount (rclone mount).

## Cài đặt

Dùng script

    curl https://rclone.org/install.sh | sudo bash

Những gì script này làm là kiểm tra loại hệ điều hành mà nó được chạy và tải xuống tệp lưu trữ liên quan đến hệ điều hành đó. Sau đó, nó trích xuất kho lưu trữ và sao chép rclone binary sang `/usr/bin/clone `và cấp quyền `755` trên tệp.

Hoặc cài thủ công

    # curl -O https://downloads.rclone.org/rclone-current-linux-amd64.zip
    # unzip rclone-current-linux-amd64.zip
    # cd rclone-*-linux-amd64
    # cp rclone /usr/bin/
    # chown root:root /usr/bin/rclone
    # chmod 755 /usr/bin/rclone
    # mkdir -p /usr/local/share/man/man1
    # cp rclone.1 /usr/local/share/man/man1/
    # mandb 

## Cấu hình

Chạy lệnh sau để tạo file config, dùng để xác thực cho việc sử dụng rclone sau này

    rclone config

Màn hình sẽ xuất hiện

[Imgur](https://i.imgur.com/ltPhxrG.png)

Các option:

- `n` – Tạo kết nối từ xa
- `s` – Đặt mật khẩu
- `q` – Thoát

Nhập `n`, nhập tên của lưu trữ tùy ý của bạn.

Sau đó chọn nền tảng lưu trữ muốn sử dụng

![Imgur](https://i.imgur.com/Tepeop4.png)

Chọn một trong số sau:

```
 1 / 1Fichier
   \ "fichier"
 2 / Alias for an existing remote
   \ "alias"
 3 / Amazon Drive
   \ "amazon cloud drive"
 4 / Amazon S3 Compliant Storage Provider (AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Minio, etc)
   \ "s3"
 5 / Backblaze B2
   \ "b2"
 6 / Box
   \ "box"
 7 / Cache a remote
   \ "cache"
 8 / Citrix Sharefile
   \ "sharefile"
 9 / Dropbox
   \ "dropbox"
10 / Encrypt/Decrypt a remote
   \ "crypt"
11 / FTP Connection
   \ "ftp"
12 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
13 / Google Drive
   \ "drive"
14 / Google Photos
   \ "google photos"
15 / Hubic
   \ "hubic"
16 / In memory object storage system.
   \ "memory"
17 / Jottacloud
   \ "jottacloud"
18 / Koofr
   \ "koofr"
19 / Local Disk
   \ "local"
20 / Mail.ru Cloud
   \ "mailru"
21 / Mega
   \ "mega"
22 / Microsoft Azure Blob Storage
   \ "azureblob"
23 / Microsoft OneDrive
   \ "onedrive"
24 / OpenDrive
   \ "opendrive"
25 / OpenStack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
26 / Pcloud
   \ "pcloud"
27 / Put.io
   \ "putio"
28 / QingCloud Object Storage
   \ "qingstor"
29 / SSH/SFTP Connection
   \ "sftp"
30 / Sugarsync
   \ "sugarsync"
31 / Tardigrade Decentralized Cloud Storage
   \ "tardigrade"
32 / Transparently chunk/split large files
   \ "chunker"
33 / Union merges the contents of several upstream fs
   \ "union"
34 / Webdav
   \ "webdav"
35 / Yandex Disk
   \ "yandex"
36 / http Connection
   \ "http"
37 / premiumize.me
   \ "premiumizeme"
38 / seafile
   \ "seafile"
```

Ở đây tôi chọn Google drive (số 13)

![Imgur](https://i.imgur.com/Hi8OiDO.png)

Xác thực:

Ở đây chúng ta cần nhập google application client ID và secret. Để trống 2 phần này (default) và tiếp tục. Nếu muốn tự tạo client ID và secret thì làm theo hướng dẫn ở dưới

Hướng dẫn tạo ở đây: https://rclone.org/drive/#making-your-own-client-id

1. Truy cập https://console.developers.google.com/
2. Tạo một project hoặc chọn một project có sẵn

![Imgur](https://i.imgur.com/NstNnh9.png)

3. Ở phần ENABLE APIS AND SERVICES. Tìm Google Drive API và enable nó lên

![Imgur](https://i.imgur.com/3fDgtad.png)

4. Tạo credential, Chọn Create OAuth client ID

![Imgur](https://i.imgur.com/ZHLOkLY.png)

5. Nếu đã cấu hình Oauth Consent Screen thì bỏ qua bước này. Nếu chưa thì Click CONFIGURE CONSENT SCREEN. Chon External và click CREATE 

![Imgur](https://i.imgur.com/kXbYvhJ.png)

![Imgur](https://i.imgur.com/48ow6x9.png)

Đến màn hình tiếp theo, đặt tên cho Application name là rclone (tùy ý). Các thông tin khác đề là tùy chọn. Sau đó click Save.

![Imgur](https://i.imgur.com/CuoxSE8.png)

6. Click lại Credentials. Click + CREATE CREDENTIALS. Click OAuth client ID

7. Chọn Desktop app. Click CREATE

8. Như vậy là ta có Client ID và Client secret. Dùng thông tin này nhập vào rclone

![Imgur](https://i.imgur.com/uQZaPT4.png)

Tiếp theo cấp quyền truy cập cho rclone tùy ý của bạn. Tôi chọn 1 (Full quyền)

![Imgur](https://i.imgur.com/cV2jmfK.png)

Các phần khác chọn default

Không sửa advanced config (n)

Sau đó chọn sử dụng auto config (y)

Một cửa sổ sẽ bật lên ở trình duyệt web của bạn để xác thực.

Ở đây tôi không thể xác thực trên Centos 7 minimal do không có trình duyệt web, nên tôi sẽ thực hiện cài rclone trên HĐH Windows. Sau khi cấu hình xong trên windows sẽ copy cấu hình lên Centos 7

![Imgur](https://i.imgur.com/SSdTGcq.png)

![Imgur](https://i.imgur.com/6JFzK40.png)

Sau khi xác thực, tiếp tục chọn n

![Imgur](https://i.imgur.com/d0gY33B.png)

Sau đó copy file cấu hình lên Centos 7. 

Tải file cài đặt ở đây: https://rclone.org/downloads/

!![Imgur](https://i.imgur.com/haNhG4I.png)

Giải nén file đã tải về. Bật CMD, vào thư mục đã giải nén và sử dụng như trên Linux.

Chạy lệnh sau để lấy đường dẫn file config, sau đó copy sang máy Centos 7. Ghi đè vào file config trên máy Centos 7

    rclone config file

Phân quyền và đặt chủ sở hữu

    chown root:root /root/.config/rclone/rclone.conf
    chmod 775 /root/.config/rclone/rclone.conf

Ở đây tôi đã thêm 3 dropbox, google drive, onedrive. 

![Imgur](https://i.imgur.com/RZGwqQL.png)

Sau khi chuyển file cấu hình sang Centos 7

![Imgur](https://i.imgur.com/m2jtLGj.png)

## Sử dụng

### Liệt kê danh sách thư mục

Liệt kê danh sách thư mục bằng lệnh:

    rclone lsd <remote-dir-name>:

Ví dụ:

    rclone lsd dropbox:

![Imgur](https://i.imgur.com/m2jtLGj.png)

### Liệt kê các file bên trong thư mục
```
rclone ls <remote-dir-name>:<another-dir>
```

### Copy dữ liệu:

Cú pháp:

    rclone copy source:sourcepath dest:destpath

Ví dụ:

Tạo một file dungdb.txt trong thư mục test one drive.

Copy thư mục test từ one drive sang google drive

    rclone copy odrive:test ggdrive:test

Kết quả:

![Imgur](https://i.imgur.com/TWsfBdW.png)

Ví dụ copy file trong thư mục test ở google drive sang thư mục /tmp máy Centos 7
```
rclone copy ggdrive:test1 /tmp/
```
![Imgur](https://i.imgur.com/rVSNyh9.png)

### Đồng bộ dữ liệu

Dùng lệnh sync
```
rclone sync source:path dest:path [flags]
```
Ví dụ:

Đồng bộ các file trong thư mục Recordings ở dropbox sang thư mục test1 ở google drive
```
rclone sync dropbox:Recordings ggdrive:test1
```

Vì lệnh đông bộ này có thể làm mất dữ liệu nên ta cần cẩn thận khi sử dụng. 

Dùng thêm flag `--dry-run` để thấy rõ file nào **SẼ** được copy và bị xóa

![Imgur](https://i.imgur.com/Fm1sYcC.png)

Nếu muốn đồng bộ dữ liệu từ VPS sang nơi lưu trữ. Nên nén dữ liệu trước khi đồng bộ.

### Di chuyển dữ liệu

Dùng lệnh move
```
rclone move source:path dest:path [flags]
```

### Tạo thư mục ở đích
```
rclone mkdir remote:path
```

Tạo thư mục ví dụ ở google drive
```
rclone mkdir ggdrive:vidu:
```

![Imgur](https://i.imgur.com/oeGjySz.png)

### Xóa thư mục
```
rclone rmdir remote:path
```

Xóa thư mục vidu
```
rclone rmdir ggdrive:vidu:
```

![Imgur](https://i.imgur.com/lfpQOF6.png)

### Kiểm tra xem các file ở nguồn và đích đã giống nhau hay chưa
```
rclone check source:path dest:path
```

![Imgur](https://i.imgur.com/E5kqCyD.png)

### Xóa file
```
rclone delete remote:path
```

Mỗi lệnh rclone để có thể dùng với các flag khác nhau. Xem các flag ở help menu. 
```
rclone --help
```

Ví dụ muốn xóa tất cả file có dung lượng lớn hơn 100MB, ta dùng lệnh:
```
rclone --min-size 100M delete remote:path
```

Ví dụ xóa các file có dung lượng nhỏ hơn 100MB trong thư mục test1
```
rclone --max-size 100M delete ggdrive:test1
```

Đọc thêm về các lệnh rclone tại đây: https://rclone.org/docs/

## Tham khảo:

https://rclone.org/

https://www.tecmint.com/rclone-sync-files-from-cloud-storage/
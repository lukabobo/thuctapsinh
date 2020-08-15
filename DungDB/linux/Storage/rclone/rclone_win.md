# Cài đặt và sử dụng trên Windows

https://rclone.org/downloads/

![Imgur](https://i.imgur.com/haNhG4I.png)

Mở CMD, vào thư mục đã giải nén rclone và chạy lệnh:

    rclone config

Các option:

- `n` – Tạo kết nối từ xa
- `s` – Đặt mật khẩu
- `q` – Thoát

Nhập `n`, nhập tên của lưu trữ tùy ý của bạn.

Sau đó chọn nền tảng lưu trữ muốn sử dụng, chọn 1 trong số dưới đây:

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

Ví dụ chọn dropbox thì nhập vào số 9.

Các lựa chọn khác cứ bấm Enter để chọn default. Sẽ có một đường dẫn mở ra ở trình duyệt web. Đăng nhập vào tài khoản dropbox của bạn và cho phép rclone sử dụng các quyền nó yêu cầu.

Quay trở lại màn hình cmd. 

Các tùy chọn khác bấm Enter để chọn mặc định

Tiếp theo cấp quyền truy cập cho rclone tùy ý của bạn. Tôi chọn 1 (Full quyền)

Bấm `y` để xác nhận.

![Imgur](https://i.imgur.com/eX0Va4d.png)

Ở đây tôi đã thêm 3 dropbox, google drive, onedrive

Liệt kê danh sách thư mục bằng lệnh:

    rclone lsd <remote-dir-name>:

Ví dụ:

    rclone lsd dropbox:

![Imgur](https://i.imgur.com/RZGwqQL.png)

## Cài đặt

Tải về và cài đặt một phần mêm giải nén nếu hệ thống chưa có trước đó

    yum install unzip -y

Cài rclone bằng script

    curl https://rclone.org/install.sh | sudo bash

## Cấu hình rclone kết nối với Dropbox

Sau khi đã cài đặt xong, tiến hành cấu hình rclone

    rclone config

Chọn n để thêm kết nối mới

![Imgur](https://i.imgur.com/CO0eXfI.png)

Đặt tên cho kết nối

![Imgur](https://i.imgur.com/re7jixs.png)

Màn hình sẽ xuất hiện các cloud storage để ta lựa chọn. Chọn số 9 của dropbox

![Imgur](https://i.imgur.com/SSfzvzT.png)

![Imgur](https://i.imgur.com/rX7iIFF.png)

Tiếp theo, ta sẽ bỏ trống mục `client_id` và nhấn Enter. Tương tự với `client_secret`.

![Imgur](https://i.imgur.com/30NWoem.png)

Ở phần `Edit advanced config` chọn **n** và Enter

![Imgur](https://i.imgur.com/qLxlGp6.png)

Ở phần `Remote config` chọn **n** và Enter

![Imgur](https://i.imgur.com/hsokLUg.png)

Chúng ta sẽ thấy có thông báo rằng cần phải có một máy có trình duyệt web đã cài rclone (khuyến cáo cài cùng phiên bản với máy ta đang sử dụng) mới có thể lấy token để tạo kết nối được.

![Imgur](https://i.imgur.com/hypkc5Z.png)

Chúng ta tạm dừng ở bước này và chuyển sang thao tác trên một máy tính có  trình duyệt web. Ở đây tôi sử dụng một máy chạy Windows 10.

Mở trình duyệt web và đăng nhập vào tài khoản dropbox ta cần liên kết với rclone

https://www.dropbox.com/login

Tải rclone ở đây: https://downloads.rclone.org/v1.52.3/rclone-v1.52.3-windows-amd64.zip

Tôi giải nén ra ổ `C:\`

![Imgur](https://i.imgur.com/aSg3xq3.png)

Mở CMD và cd vào thư mục chứa rclone

![Imgur](https://i.imgur.com/zNiZcPx.png)

Gõ lệnh 
    
    rclone authorize "dropbox"

![Imgur](https://i.imgur.com/Mv17Sqd.png)

Sẽ có một cửa sổ web bật lên ở trình duyệt web. Chúng ta click vào nút Allow

![Imgur](https://i.imgur.com/WJlpCB7.png)

Sau đó ta sẽ thấy thông báo thành công

![Imgur](https://i.imgur.com/uJY1qqP.png)

Quay trở lại cửa số CMD. Chúng ta đã lấy được token. 

![Imgur](https://i.imgur.com/GBM6YQV.png)

Copy dòng này và paste vào máy linux ở bước ta tạm dừng lúc nãy.

![Imgur](https://i.imgur.com/XOjXrXt.png)

Bước tiếp theo, chọn **y** và Enter để xác nhận

![Imgur](https://i.imgur.com/GTncEzy.png)

Như vậy là ta đã tạo được kết nối dropbox với rclone. Nhấn **q** để thoát.

![Imgur](https://i.imgur.com/uFEU3uj.png)

## Sử dụng rclone để copy/sync file lên Dropbox

Để copy từ máy local sang remote storage, ta sử dụng câu lệnh:

    rclone copy /local/path remote:path

Ví dụ tôi tạo 1 file ở VPS của mình

    touch testcopy.txt

Sau đó copy lên Dropbox bằng câu lệnh sau:

    rclone copy testcopy.txt dropbox:

Kiểm tra trên Dropbox sẽ thấy có file.

![Imgur](https://i.imgur.com/WTTbJCt.png)

Ngoài ra bạn có thể đồng bộ cả một thư mục lên Dropbox bằng câu lệnh sau:

rclone sync /local/path remote:path
Bằng cách này, ta có thể viết một số script rồi đặt trong crontab để backup các file mà ta mong muốn hàng ngày lên Dropbox.

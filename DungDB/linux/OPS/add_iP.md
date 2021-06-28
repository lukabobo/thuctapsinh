# Thêm IP vào horizon 

## Thêm IP vào dải đã có

Đăng nhập vào horizon. Chọn project admin. 

Tại phần Admin/Network/Networks. Click vào 	Network Name của dải IP cần khai báo thêm IP

Click vào Edit subnet

![Imgur](https://i.imgur.com/UHbt2hV.png)

Click vào subnet details

![Imgur](https://i.imgur.com/el1aOkb.png)

Điền IP cần thêm vào pool và click Save.

Ví dụ cần thêm 1 IP là 1.2.3.4 thì khai vào pool: 1.2.3.4,1.2.3.4

Ví dụ cần khai 1.2.3.4, 1.2.3.5, 1.2.3.6 thì khai vào pool: 1.2.3.4,1.2.3.6

## Thêm IP vào dải chưa có

Đăng nhập vào horizon. Chọn project admin. 

Tại phần Admin/Network/Networks Click nút Create Network

![Imgur](https://i.imgur.com/PrjsHWx.png)

Điền các thông tin như sau

![Imgur](https://i.imgur.com/XOfTlg6.png)

Next

![Imgur](https://i.imgur.com/ctoWvTV.png)

Nếu là dải VAS (IP khách yêu cầu thêm thì không cần khai báo gateway). Tick vào Disable gateway.

Nếu là dải IP public của mình thì khai báo gateway

Khai báo pool và DNS. Sau đó click Create

![Imgur](https://i.imgur.com/y9GyOZm.png)
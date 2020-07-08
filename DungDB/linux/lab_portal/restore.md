# Lab restore ổ cứng trên portal

Hướng dẫn: https://kb.nhanhoa.com/pages/viewpage.action?pageId=37453910

VPS cần restore: 103.124.94.51

SSH vào VPS tắt dịch vụ httpd

![Imgur](https://i.imgur.com/8V1a056.png)

Lấy volume ID

![Imgur](https://i.imgur.com/CIrsexC.png)

![Imgur](https://i.imgur.com/190uZaO.png)

Tìm file backup

![Imgur](https://i.imgur.com/kcjL7os.png)

Có 2 bản backup có sẵn ở đây.

![Imgur](https://i.imgur.com/d1604hG.png)

![Imgur](https://i.imgur.com/AjqS8ZK.png)

Khi restore mà không tắt máy sẽ fail

![Imgur](https://i.imgur.com/EoJeP5C.png)

![Imgur](https://i.imgur.com/l3eqcf8.png)

Tắt máy ảo và làm lại

Hoàn thành

![Imgur](https://i.imgur.com/EN4SytV.png)

Danh sách job

![Imgur](https://i.imgur.com/Sa7IZuF.png)

Sau khi restore thì dịch vụ web đã quay về trạng thái cũ (bật)

![Imgur](https://i.imgur.com/GABPNeJ.png)

	9bece97a-32dd-44bc-82ff-c701bf154339


Tạo một VPS mới, có IP sau: 103.124.92.233

Tắt VPS 103.124.92.233 đi để backup dữ liệu từ VPS 103.124.94.51

Nhập volumeID của VPS 103.124.94.51

![Imgur](https://i.imgur.com/AjzsHs9.png)

Chọn bản backup

![Imgur](https://i.imgur.com/5HaI0VQ.png)

Nhập volumeID của VPS 103.124.92.233

![Imgur](https://i.imgur.com/Os6ZKwc.png)

![Imgur](https://i.imgur.com/qtgHVHF.png)

![Imgur](https://i.imgur.com/puptKzx.png)

![Imgur](https://i.imgur.com/NgAxdeE.png)

![Imgur](https://i.imgur.com/Q1jtF6Y.png)

Dữ liệu đã được back up

![Imgur](https://i.imgur.com/he3U3hY.png)
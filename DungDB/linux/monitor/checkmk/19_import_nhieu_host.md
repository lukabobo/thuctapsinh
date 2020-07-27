# Cách import nhiều host cùng lúc bằng file .csv

Tạo 1 file excel. Nhập vào nội dung các host bạn cần giám sát

Ví dụ nội dung như sau:

![Imgur](https://i.imgur.com/V9dyoiU.png)

`Save as`. Lưu file này lại với định dạng .csv

Vào trang checkmk. Chọn thư mục cần thêm host để giám sát và click `Bulk import`

Chọn file csv vừa tạo và upload

Chúng ta sẽ thấy hiển thị như sau:

![Imgur](https://i.imgur.com/0OZGety.png)

Chọn nội dung hiển thị cho từng cột. 

![Imgur](https://i.imgur.com/bY0rb3p.png)

Ví dụ này tôi để cột 1 là IP. Cột 2 là Hostname. Cột 3 là Alias

![Imgur](https://i.imgur.com/hg1ua8X.png)

Sau đó click import

![Imgur](https://i.imgur.com/fRPxGk9.png)

Các host được thêm:

![Imgur](https://i.imgur.com/50Vz4aH.png)

Bây giờ chỉ cần active các thay đổi là ta đã thêm thành công nhiều host trong file csv một lúc để giám sát với checkmk.

![Imgur](https://i.imgur.com/UkFsD3V.png)

![Imgur](https://i.imgur.com/UBjsJRf.png)

Tham khảo: https://github.com/niemdinhtrong/thuctapsinh/blob/master/NiemDT/Ghichep_checkmk/docs/08.Add-nhieu-host.md
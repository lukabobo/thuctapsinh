# 1 số thao tác cơ bản

## Giám sát tài nguyên

Tài nguyên hiện tại:

![Imgur](https://i.imgur.com/8umWg3E.png)

Ở cột Vcpus Total.

Tổng số lượng v-cpu có thể cấp phát là con số ở đây x4. Chúng ta chỉ cấp phát 80% con số này.

Ví dụ COM1 có 24 v-cpu total. Số lượng v-cpu tối đa COM1 có thể cấp phát là 24x4=96. 96x80%=76.8. Vậy ta chỉ cấp phát 77 vpcu trên COM1.

Nhìn vào hình trên có thể thấy COM1 đang sử dụng quá 5 v-cpu. Sẽ không tạo VPS vào COM1 nữa.

COM2 và COM4 đang khá thoải mái tài nguyên. Có thể tạo VPS vào đây.

COM3 đang sử dụng vượt mức cho phép. Không thể tạo thêm VPS vào COM3. Cần migrate (chuyển) các VPS ở đây sang COM2 hoặc COM4.

## Tạo chỉ định VPS vào node COM cụ thể

Bây giờ ta không tạo thêm VPS vào COM1 và COM3 nữa. Nên khi có yêu cầu tạo VPS mới. Ta truy cập vào horizon, tắt dịch vụ COM1 và COM3 đi. Sau đó tạo mới VPS. Sau khi tạo xong VPS thì bật lại dịch vụ cho COM1 và COM3.

Thực hiện như sau:

Truy cập vào horizon. Tại phần Admin/ Compute/ All Hypervisors -> Compute host

![Imgur](https://i.imgur.com/r976qqM.png).

Click vào nút disable Service tại COM1 và COM3.

Sau đó truy cập portal tạo VPS như bình thường. VPS sẽ tự động được tạo vào COM2 hoặc COM4.

Sau khi tạo xong VPS. Quay lại horizon. Click vào nút Enable service tại COM1 và COM3.

## Xóa volume

Truy cập horizon. Ở phần Admin/ Volume/ Volumes

![Imgur](https://i.imgur.com/CVc7AIx.png)

Có thể xóa các volume đang ở trạng thái available. Click vào nút Delete volume.

Sau khi xóa volume sẽ không thể khôi phục lại VPS nữa.

## Migrate VM tới 1 com chỉ định

Ví dụ tôi muốn migrate VM từ COM2 sang COM4.

Truy cập vào horizon. Tại phần Admin/ Compute/ All Hypervisors -> Compute host

Click vào nút disable Service tại COM1 và COM3.

Sau đó tắt VM. Rồi chọn Migrate instance như hình dưới

![Imgur](https://i.imgur.com/qw193Hn.png)

Sau đó click comfirm

![Imgur](https://i.imgur.com/1R8KMOz.png)

VM đã sang COM4

![Imgur](https://i.imgur.com/1WgiOBM.png)

Sau khi migrate xong, bật lại dịch vụ COM1 và COM3 tại phần Admin/ Compute/ All Hypervisors -> Compute host 

Khởi động lại VM để kiểm tra.


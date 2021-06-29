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

## Sử dụng backy2 để restore VM

Click vào VM cần restore

![Imgur](https://i.imgur.com/MqBsxHu.png)

Click vào Volume

![Imgur](https://i.imgur.com/BzaULoP.png)

Ta thấy được ID volume

![Imgur](https://i.imgur.com/hRUS9Q9.png)

Lưu ý: Đây là ID volume của OpenStack, ở phía dưới Ceph, volume sẽ có tên là **volume-xxx**

Ví dụ với ID volume trên, ta sẽ có tên của volume dưới ceph là **volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664**

Trên server backup, ta có thể kiểm tra các bản backup của volume này bằng câu lệnh

    backy2 ls | grep volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664

Kiểm tra thông tin dưới ceph về thông tin của volume

    rbd info volumes/volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664

![Imgur](https://i.imgur.com/lsteK04.png)

Để backup volume này, ta có thể thực hiện câu lệnh sau

    backy2 backup -t <tag> rbd://volumes/volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664 volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664

Trong đó:

- tag: là tag bạn muốn gán cho bản backup, có thể là ngày (vd 19082020) để phục vụ việc query cho sau này.

Quá trình backup sẽ bắt đầu chạy.

**Lưu ý:** Backup cho phép chạy khi VM đang running, tuy nhiên việc này có thể gây lỗi với hệ điều hành sau khi restore. Vì thế hay xem xét tắt VM trước khi backup trong trường hợp cần thiết.

**Để restore volume về thời điểm backup**

Trước tiên ta cũng sẽ lấy thông tin của volume cần restore (xem lại các bước trên).

Sau khi có được volume ID, ta sẽ tiến hành check xem volume đó đang có những bản backup nào. Ví dụ:

    backy2 ls | grep volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664

![Imgur](https://i.imgur.com/pPrhV2Q.png)

![Imgur](https://i.imgur.com/dGGY8k5.png)

Trong hình ta cần lưu ý 1 số:

1 - thời gian thực hiện backup, lưu ý đây là thời gian theo UTC

2 - backup ID

3 - backup có valid hay không. Lưu ý ta chỉ có thể restore những bản backup có trường valid là 1

Sau khi xác định được ID của bản backup cần restore. Ta sẽ thực hiện tắt VM cần restore.

Lưu ý đảm bảo VM đã được tắt trước khi thực hiện các thao tác tiếp theo.

Xem thông tin của volume

Sau khi xác định được ID của bản backup cần restore. Ta sẽ thực hiện tắt VM cần restore.

Lưu ý đảm bảo VM đã được tắt trước khi thực hiện các thao tác tiếp theo.

Xem thông tin của volume

    backy2 ls | grep volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664

Xóa volume, trước khi xóa cần shutdown máy.

    rbd rm volumes/volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664

Bật byobu

    byobu

Restore volume

    backy2 restore 9825cd22-d69d-11eb-a27e-141877630cba rbd://volumes/volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664

Trong đó:

`9825cd22-d69d-11eb-a27e-141877630cba` là ID của bản backup ta muốn restore về

`volume-5a46aafe-c7b3-4faf-9c04-9251be7b7664` là ID của volume ta muốn restore

![Imgur](https://i.imgur.com/lquxAaU.png)

![Imgur](https://i.imgur.com/zoh7uto.png)

Chờ đợi backup chạy. Quá trình này phụ thuộc vào dung lượng của volume.

**Backup ra một VM khác**

Ngữ cảnh: Restore một VM đã xóa hoặc restore ra VM khác phục vụ cho trường hợp cần check.

Trước tiên ta cũng sẽ lấy thông tin của volume. Từ đó lấy thông tin của bản backup.

Sau đó ta sẽ tạo 1 máy ảo mới với cấu hình giống với máy ảo cũ. Tiếp tục lấy thông tin volume của máy ảo mới.

Tiến hành tắt máy ảo mới tạo. Sau đó xóa volume của máy ảo mới.

Cuối cùng là restore bản backup về volume của máy ảo mới. Các thao tác tương tự như 

## Giới hạn băng thông VPS

Thao tác trên controller

Xác định id của project cần set

![Imgur](https://i.imgur.com/eWyoc2G.png)

Chạy lệnh

    source admin-openrc

Xác định port của IP vm cần set qos

    openstack port list | grep 14.225.16.40

![Imgur](https://i.imgur.com/eakVBvP.png)

`43272e1a-420b-458b-b165-1e816e202b94`

Tạo policy

    openstack network qos policy create dung-demo-interface-in-and-out-200Mb --project 01ff6af7d1df4fd69a39a2ada74315dc --share

Trong đó:

- `dung-demo-interface-in-and-out-200Mb` là tên của policy

- `01ff6af7d1df4fd69a39a2ada74315dc` là id của project

Tạo Qos policy giới hạn cả incoming và outgoing traffic của vm với mức 200Mbps trong project

    openstack network qos rule create --type bandwidth-limit --max-kbps 100000 --max-burst-kbits 80000 --egress dung-demo-interface-in-and-out-200Mb

    openstack network qos rule create --type bandwidth-limit --max-kbps 100000 --max-burst-kbits 80000 --ingress dung-demo-interface-in-and-out-200Mb

set rule cho port vm:

    openstack port set --qos-policy dung-demo-interface-in-and-out-200Mb 43272e1a-420b-458b-b165-1e816e202b94


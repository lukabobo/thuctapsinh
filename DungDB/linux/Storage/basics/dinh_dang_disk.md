# Định dang disk

https://cloudcraft.info/huong-dan-toan-tap-ve-partition-tren-linux/

## Giới thiệu về partition

Partition là những phân vùng nhỏ (phân vùng logic) được chia ra từ 1 ổ cứng vật lý. Một ổ cứng có thể có 1 hoặc nhiều partition. Partition là cách phân chia và quản lý một ổ cứng đơn giản và hiệu quả (chẳng hạn như phân ra 1 vùng quan trọng để chứa dữ liệu của hệ điều hành và 1 phân vùng để chứa phim, nhạc).

Dữ liệu trên 1 partition A sẽ được phân tách với dữ liệu trên partition B, mọi thao tác trên partition này sẽ không ảnh hưởng đến partition kia (trừ khi ổ cứng chung bị hư).

Hiện có 3 loại partition chính là: primary, extended và logical.

- Primary partition: đây là những phân vùng có thể được dùng để boot hệ điều hành
- Extended partition: là vùng dữ liệu còn lại khi ta đã phân chia ra các primary partition, extended partition chứa các logical partition trong đó. Mỗi một ổ đĩa chỉ có thể chứa 1 extended edition.
- Logical partition: các phân vùng nhỏ nằm trong extended partition, thường dùng để chứa dữ liệu.

### MBR vs GPT
Thông tin về các partition của ổ cứng sẽ được lưu trữ trên MBR (Master Boot Record) hoặc GPT (GUID Partition Table) tùy loại ổ cứng hỗ trợ. Đây là 2 chuẩn cấu hình và quản lý các partition trên ổ cứng. Thông tin được lưu trữ trên đây gồm vị trí và dung lượng của các partition.

#### MBR

MBR được giới thiệu lần đầu tiên với IBM PC DOS 2.0 vào năm 1983. MBR chứa thông tin về cách phân vùng logical chứa các hệ thống tệp được sắp xếp trên đĩa. Nó chứa code thực thi(bộ tải khởi động) để hoạt động như một trình tải cho hệ điều hành được cài đặt. MBR cũng chỉ hỗ trợ tối đa bốn phân vùng chính, nếu bạn muốn nhiều hơn, bạn phải tạo một trong các phân vùng chính của mình thành một phân vùng mở rộng của Wap và tạo các phân vùng hợp lý bên trong nó. MBR sử dụng 32 bit để lưu trữ địa chỉ khối và đối với các đĩa cứng có các sectors 512 byte, MBR xử lý tối đa 2TB (2^32 × 512 byte).

MBR là chuẩn phân chia ổ đĩa truyền thống, một ổ đĩa sẽ được chia thành các vùng nhỏ (sector) với dung lượng bằng nhau là 512 bytes. Trên Linux, một ổ đĩa cứng được chia thành nhiều partition với số hiệu như sau: `/dev/hda1`, `/dev/hda2`, `/dev/sda1`, `/dev/sda2`, `/dev/sdb1`,… Ta có thể dùng lệnh fdisk hoặc parted để hiển thị thông tin về ổ đĩa dùng MBR trên Linux.

Đối với các ổ cứng kiểu cũ chỉ hỗ trợ MBR thì ta chỉ được phép có tối đa 4 primary partition trên 1 ổ cứng, extended partion cũng được coi là 1 primary partition. Toàn bộ các thông tin về partition sẽ được lưu trữ ở 512 bytes đầu tiên trên ổ đĩa vật lý (sector đầu tiên của ổ đĩa), sector này có tên là Master Boot Record.

Định dạng MBR  => có nghĩa là máy tính đang chạy ở chuẩn LEGACY.

#### GPT

GPT là chuẩn mới hơn, hỗ trợ đến 128 phân vùng trên 1 ổ đĩa vật lý. Thông tin về các partition sẽ được ghi thành nhiều bản rải rác khắp ổ vật lý. GPT hỗ trợ cơ chế kiểm tra và chỉnh sửa dữ liệu dựa trên CRC (cyclic redundancy check). Nhờ 2 cơ chế này, chuẩn GPT làm giảm tỷ lệ mất mát dữ liệu. Ngoài ra, nếu ta cần khởi tạo một phân vùng với dung lượng lớn hơn 2TB, ta sẽ phải dùng GPT vì MBR không trợ dung lượng lớn hơn 2 TB.

GPT có thể có 128 phân vùng. GPT sử dụng 64 bit cho địa chỉ khối và cho các đĩa cứng có các sectors 512 byte, kích thước tối đa là 9,4 ZB (9,4 × 10^21 byte) hoặc 8ZiB.

Ta có thể dùng lệnh `gdisk` hoặc `parted` để kiểm tra các ổ đĩa dùng GPT.

Định dạng GPT => có nghĩa là máy tính đang chạy ở chuẩn UEFI.

## Quy trình lắp mới một thiết bị lưu trữ gồm những bước sau:

- Xác định chuẩn lưu thông tin về partition (MBR, GPT).
- Gắn một ổ cứng mới vào server.
- Chia partition cho ổ cứng.
- Khởi tạo file system (ext4, xfs…) cho partition vừa mới được tạo.
- Mount partition lên hệ thống.
- Cấu hình auto mount partition khi reboot.
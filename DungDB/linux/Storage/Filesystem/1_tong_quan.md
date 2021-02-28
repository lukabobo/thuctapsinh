# File system

## Khái niệm

Filesystem là các phương pháp và các cấu trúc dữ liệu mà một hệ điều hành sử dụng để theo dõi các tập tin trên ổ đĩa hoặc phân vùng. Có thể tạm dịch filesystem là hệ thống tập tin. Đó là cách các tập tin được tổ chức trên ổ đĩa. Filesystem được sử dụng để kiểm soát dữ liệu, lưu trữ và lấy lại. Nếu không có filesystem, thông tin được lưu trong nhưng khối lớn sẽ không có cách nào để tìm thấy vị trí bắt đầu và vị trí kết thúc, vị trí tiếp theo của dữ liệu. Bởi dữ liệu được chia thành các mảnh và mỗi mảnh được đặt tên, vì thế thông tin dễ dàng được đánh dấu và xác thực.

Có rất nhiều loại file system. Mỗi loại đều có cấu trúc và logic khác nhau, tính chất về tốc độ, tính linh hoạt, bảo mật, size ... 1 số file system được thiết kể để sử dụng cho 1 số ứng dụng đặc biệt. Ví dụ, ISO 9660 file system được thiết đặc biệt cho đĩa quang. File system có thể sử dụng nhiều trên loại thiết bị lưu trữ và các loại phương tiên truyền thống khác nhau. Nổi bật và thông thường nhất là ổ đĩa cứng. Bên cạnh đó là bộ nhớ flash, đĩa quang. Trong 1 số trường hợp, như tmpfs, computer main memory (RAM) sử dụng cả file đệm, sử dụng trong thời gian ngắn. 1 số file system được sử dụng cho local data storage devices, bên cạnh đó cung cấp truy chế truy cập file thông qua giao thức mạng (NFS, SMB, 9P). Một số file system là ảo, có nghĩa cung cấp “file” ảo sử dụng cho những yêu cấu tính toán hoặc ánh xạ vào nhưng file system khác. File system quản lý truy cập trên cả nội dung của file và metadata của nhưng file này. Nó chịu trách nhiệp sắp xếp không gian lưu trữ đảm bảo, tin cậy, rõ ràng, có hệ thống.

## Các thư mục trong Linux:

Filesystem của hệ điều hành Linux được tổ chức theo tiêu chuẩn cấp bậc của hệ thống tập tin Filesystem Hierarchy Standard ( FHS ). Tiêu chuẩn này định nghĩa mục đích của mỗi thư mục.

1. `/` - Root
Đây là nơi bắt đầu của tất cả các file và thư mục. Chỉ có root user mới có quyền ghi trong thư mục này. Chú ý rằng /root là thư mục home của root user chứ không phải là /.
2. `/bin` - Chương trình của người dùng
Thư mục này chứa các chương trình thực thi. Các chương trình chung của Linux được sử dụng bởi tất cả người dùng được lưu ở đây. Ví dụ như: ps, ls, ping...
3. `/sbin` - Chương trình hệ thống
Cũng giống như /bin, /sbin cũng chứa các chương trình thực thi, nhưng chúng là những chương trình của admin, dành cho việc bảo trì hệ thống. Ví dụ như: reboot, fdisk, iptables...
4. `/etc` - Các file cấu hình
Thư mục này chứa các file cấu hình của các chương trình, đồng thời nó còn chứa các shell script dùng để khởi động hoặc tắt các chương trình khác. Ví dụ: `/etc/resolv.conf`, `/etc/logrolate.conf`
5. `/dev` - Các file thiết bị
Các phân vùng ổ cứng, thiết bị ngoại vi như USB, ổ đĩa cắm ngoài, hay bất cứ thiết bị nào gắn kèm vào hệ thống đều được lưu ở đây. Ví dụ: /dev/sdb1 là tên của USB bạn vừa cắm vào máy, để mở được USB này bạn cần sử dụng lệnh mount với quyền root: `mount /dev/sdb1 /tmp`
6. `/tmp` - Các file tạm
Thư mục này chứa các file tạm thời được tạo bởi hệ thống và các người dùng. Các file lưu trong thư mục này sẽ bị xóa khi hệ thống khởi động lại.
7. `/proc` - Thông tin về các tiến trình
Thông tin về các tiến trình đang chạy sẽ được lưu trong /proc dưới dạng một hệ thống file thư mục mô phỏng. Ví dụ thư mục con /proc/{pid} chứa các thông tin về tiến trình có ID là pid (pid ~ process ID). Ngoài ra đây cũng là nơi lưu thông tin về về các tài nguyên đang sử dụng của hệ thống như: /proc/version, /proc/uptime...
8. `/var` - File về biến của chương trình
Thông tin về các biến của hệ thống được lưu trong thư mục này. Như thông tin về log file: /var/log, các gói và cơ sở dữ liệu /var/lib...
9. `/usr` - Chương trình của người dùng
Chứa các thư viện, file thực thi, tài liệu hướng dẫn và mã nguồn cho chương trình chạy ở level 2 của hệ thống. Trong đó
/usr/bin chứa các file thực thi của người dùng như: at, awk, cc, less... Nếu bạn không tìm thấy chúng trong /bin hãy tìm trong /usr/bin
/usr/sbin chứa các file thực thi của hệ thống dưới quyền của admin như: atd, cron, sshd... Nếu bạn không tìm thấy chúng trong /sbin thì hãy tìm trong thư mục này.
/usr/lib chứa các thư viện cho các chương trình trong /usr/bin và /usr/sbin
/usr/local chứa các chương tình của người dùng được cài từ mã nguồn. Ví dụ như bạn cài apache từ mã nguồn, nó sẽ được lưu dưới /usr/local/apache2
10. `/home` - Thư mục người của dùng
Thư mục này chứa tất cả các file cá nhân của từng người dùng. Ví dụ: /home/john, /home/marie
11. `/boot` - Các file khởi động
Tất cả các file yêu cầu khi khởi động như initrd, vmlinux. grub được lưu tại đây. Ví dụ vmlixuz-2.6.32-24-generic
12. `/lib` - Thư viện hệ thống
Chứa cá thư viện hỗ trợ cho các file thực thi trong /bin và /sbin. Các thư viện này thường có tên bắt đầu bằng ld* hoặc lib*.so.*. Ví dụ như ld-2.11.1.so hay libncurses.so.5.7
13. `/opt` - Các ứng dụng phụ tùy chọn
Tên thư mục này nghĩa là optional (tùy chọn), nó chứa các ứng dụng thêm vào từ các nhà cung cấp độc lập khác. Các ứng dụng này có thể được cài ở /opt hoặc một thư mục con của /opt
14. `/mnt` - Thư mục để mount
Đây là thư mục tạm để mount các file hệ thống. Ví dụ như # mount /dev/sda2 /mnt
15. `/media` - Các thiết bị gắn có thể gỡ bỏ
Thư mục tạm này chứa các thiết bị như CdRom /media/cdrom. floppy /media/floopy hay các phân vùng đĩa cứng /media/Data (hiểu như là ổ D:/Data trong Windows)
16. `/srv` - Dữ liệu của các dịch vụ khác
Chứa dữ liệu liên quan đến các dịch vụ máy chủ như /srv/svs, chứa các dữ liệu liên quan đến CVS.

## Các loại filesystem được Linux hỗ trợ:

- Filesystem cơ bản: EXT2, EXT3, EXT4, XFS, Btrfs, JFS, NTFS,…
- Filesystem dành cho dạng lưu trữ Flash: thẻ nhớ,…
- Filesystem dành cho hệ cơ sở dữ liệu
- Filesystem mục đích đặc biệt: procfs, sysfs, tmpfs, squashfs, debugfs,…

Để có thể truy cập dữ liệu được lưu trữ trong USB, đĩa CD/DVD, file ISO, phân vùng ổ cứng, các tài nguyên được chia sẻ qua mạng (gọi chung là thiết bị)… trong Linux thì trước hết các thiết bị này các được gắn kết (mount) vào 1 thư mục trống (gọi là mount point) đã tồn tại sẵn trong cây thư mục. Và khi muốn tháo gỡ thiết bị đang hoạt động khỏi hệ thống thì ta phải ngắt kết nối (unmount) giữa thiết bị với mount point trước.

2 công cụ giúp ta thực hiện công việc gắn kết và tháo gỡ trên là:

`mount` và `umount`

## Kiến trúc

File system bao gồm 2 hoặc 3 lớp. Đôi khi các lớp được chia rõ ràng, đôi lúc được kết hợp lại.

- Lớp thứ nhất – “logical file system”: “logical file system” chịu trách nhiệm tương tác với ứng dụng người dùng. Nó cung cấp API cho các hoạt động cơ bản – Mở, đóng, đọc, .. và truyền các yêu cầu xuống lớp dưới cho việc xử lý. “Logical file system” quản lý hoạt động mở đối tượng “file table” và “per-process file descriptors”. Lớp này cung cấp “file access, directory operations, và security and protection”.
- Lớp thứ 2 (không bắt buộc) - virtual file system. Là lớp giao diện cho phép hỗ trợ đồng thời nhiều loại file system vật lý, còn được gọi là thực thi file system.
- Lớp thứ 3 - physical file system. Đây là lớp liên quan đến hoạt động vật lý của thiết bị lưu trữ (disk). Nó xử lý các khối vật lý cho việc đọc hoặc ghi. Nó xử lý các buffer, memory management và chịu trách nhiệm bố trí các khối vật lý trong những ví trí được chỉ định. Physical file system tương tác với device drivers hoặc các kênh tới thiết bị lưu trữ.

File system chịu trách nhiệm tổ chức file và directories, chỉ định các phân vùng để lưu trữ hay không lưu trữ. Hiện tượng dư thừa các không gian lưu trữ mà không thể tận dụng được gọi là “slack space”. Độ lớn cấp phát cho các đối tượng được chọn khi file system được tạo. Việc chọn độ lớn cấp phát phụ thuộc vào độ lớn trung bình mà file cần cho việc lưu trữ. Thông thướng sẽ cấp phát hợp lý nhất cho việc lưu trữ. Khái niệm phân mảnh xảy ra khi các không gian dư thừa, không lữu trữ hoặc các file đơn không tiếp giáp. Việc này xuất hiện trong quá trình sử dụng, tạo, sửa, xóa file. Khi file được tạo mới cần không gian lớn hơn các phân mảnh hiện có(hợp lý nhất cho việc lưu trữ). Các file được xóa nhưng không gian đang sử dụng lại không được cấp phát cho việc lưu trữ

- Filename Filename được sử dụng để xác thực vị trí lưu trữ trong file system. Hầu hết file system có hạn chế về độ dài, quy tắc đặt tên “filename”.

- Directories Filesystem còn có dạng directory, cho phép user nhóm các file riêng lẻ thành 1 tập hợp. Có thể được thực hiện bằng cách gán file với với số thứ tự trong table of content hoặc inode trong hệ điều hành nhân Unix. Directory có thể cho phép tổ chức dạng phân cấp chứ nhiều file và direc bên trong.

- Metadata Thông tin đi kèm với mỗi file system. Như độ dài của dữ liệu file được lưu với số block được phân phát hoặc dung lượng. Thời gian chỉnh sửa cuối cùng. Thời gian tạo, khối, character, socket, .. Chủ sở hữu userID và group ID, quyển truy cập

## Nhiều filesystem trên 1 hệ thống

Thông thường, nhà phân phối thường cấu hình với 1 file system duy nhất trên toàn thiết bị lưu trữ. 1 số file system với thuộc tính khác nhau có thể cùng được sử dụng. VD: browser cache, được cấu hình để lưu trong nhưng bộ đệm cấp phát nhỏ, cho phép xóa, tạo mới liên tục mà không ảnh hưởng đến hệ thống lưu trữ. Bên cạnh đó, 1 số hệ thống đám mây sử dụng "disk images" sử dụng file system khác nhau. Ví dụ dễ thấy nhất là ảo hóa, user có thể chạy định dạng ext4 của Linux trên máy ảo, mà máy ảo đó được lưu trữ trên định dạng NTFS windows. Ext4 file system được định dạng lại disk image, mà disk image được lưu trên NTFS host.

## FUSE (Filesystem in Userspace)

Filesystem in Userspace (FUSE) là một lõi module nạp được cho các hệ thống máy tính chạy hệ điều hành họ Unix, cho phép người dùng thông thường có khả năng tạo các hệ thống tệp mà không cần chỉnh sửa mã kernel

Đây là kết quả đạt được khi chạy các mã hệ thống tệp trong không gian người dùng, trong khi module FUSE chỉ cung cấp "cầu nối" đến giao diện kernel thực sự. FUSE đã chính thức được tích hợp vào nhân Linux từ phiên bản 2.6.14.

FUSE rất hữu dụng trong việc ghi các hệ thống tệp ảo. Không như các kiểu hệ thống tệp khác, ghi và nhận trực tiếp dữ liệu từ đĩa, hệ thống tệp ảo không lưu trữ dữ liệu.

Phát hành dưới giấy phép GNU General Public License và GNU Lesser General Public License, FUSE là phần mềm tự do.

FUSE có các bản cho Linux, FreeBSD, NetBSD (cũng như PUFFS), OpenSolaris, và Mac OS X.

## Truy xuất filesystem

Các thông tin này cần được truy xuất và đọc vào bộ nhớ máy tính để xử lý. Thông tin trong tập tin có thể được truy xuất bằng nhiều cách.

### Truy xuất tuần tự

- Phương pháp đơn giản nhất, Thông tin trong tập tin được xử lý có thứ tự
- Chế độ truy xuất này là thông dụng nhất. VD: bộ soạn thảo và biên dịch

### Truy xuất trực tiếp

- Truy xuất trực tiếp (hay truy xuất tương đối)
- Tập tin được hình thành từ các logical records có chiều dài không đổi
- Cho phép người lập trình đọc và viết các mẫu tin nhanh chóng, không theo thứ tự
- Để truy xuất trực tiếp, tập tin được hiển thị như một chuỗi các khối hay mẫu tin được đánh số
- Phương pháp được sử dụng truy xuất tức thời lượng lớn thông tin (pp cơ sở dữ liệu).
### Các phương pháp truy xuất khác

- Được xây dựng trên cơ sở của phương pháp truy xuất trực tiếp
- Việc xây dựng chỉ mục cho tập tin. Chỉ mục chứa các con trỏ chỉ tới các khối khác
- Để tìm một mẫu tin trong tập tin - tìm chỉ mục - con trỏ để truy xuất tập tin trực tiếp và tìm mẫu tin mong muốn

# Tham khảo:

https://blogd.net/linux/tong-quan-ve-filesystem-tren-linux/

https://quantrimang.com/filesystem-la-gi-135173
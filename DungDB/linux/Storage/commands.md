# Các lệnh liên quan

## `df`

https://www.tecmint.com/how-to-check-disk-space-in-linux/

## `du`

https://www.tecmint.com/check-linux-disk-usage-of-files-and-directories/

## `mount`

https://linuxhint.com/linux_mount_command/

https://linuxize.com/post/how-to-mount-and-unmount-file-systems-in-linux/

## `fdisk`

https://www.tecmint.com/fdisk-commands-to-manage-linux-disk-partitions/

Xem tất cả phân vùng

    fdisk -l

![Imgur](https://i.imgur.com/Yk68qQm.png)

Xem tất cả các lệnh có thể dùng trong một phân vùng cụ thể

![Imgur](https://i.imgur.com/AgzqJsl.png)

- `p`: In ra màn hình thông tin các phân vùng
- `d`: Xóa phân vùng

![Imgur](https://i.imgur.com/RSO9qvX.png)

- `n`: Tạo phân vùng mới.

![Imgur](https://i.imgur.com/9zkZWwy.png)

- `w`: Lưu lại các thay đổi sau khi sửa, xóa. Sau khi lưu nên khởi động lại hệ thống

Format phân vùng vừ tạo với lệnh `mkfs`

![Imgur](https://i.imgur.com/xEkV8y1.png)

Kiểm tra dung lượng với option `-s`

![Imgur](https://i.imgur.com/aHVZD75.png)

## `mkfs`

https://www.howtogeek.com/443342/how-to-use-the-mkfs-command-on-linux/

## `rsync`

https://www.tecmint.com/rsync-local-remote-file-synchronization-commands/

https://www.tecmint.com/sync-new-changed-modified-files-rsync-linux/

https://www.tecmint.com/sync-files-using-rsync-with-non-standard-ssh-port/

### Cài đặt:

    # yum install rsync (On Red Hat based systems)
    # apt-get install rsync (On Debian based systems)

### Các option:

- `-v`: Cụ thể
- `-r`: chỉ copy file, không lưu quyền, thời gian khi chuyển dữ liệu
- `-a`: copy file, lưu toàn bộ thông tin của file như quyền, symbolic link, thời gian, quyền sở hữu
- `-z`: nén
- `-h`: chế độ dễ đọc cho người dùng

### Ví dụ sync file local

Tạo một file dung.txt

![Imgur](https://i.imgur.com/OEn8Unb.png)

sync file trên tới /tmp/dung.txt

![Imgur](https://i.imgur.com/QutMVfX.png)

### Ví dụ sync file từ xa

Đứng ở host 10.10.34.177 sync file dung.txt sang host 10.10.34.175

    rsync -arvz -e 'ssh -p 22' dungb.txt root@10.10.34.175:/tmp/dung.txt

![Imgur](https://i.imgur.com/WO7C7D4.png)

Kiểm tra ở host 10.10.34.175

![Imgur](https://i.imgur.com/S15lhk9.png)

## `parted`

https://www.tecmint.com/parted-command-to-create-resize-rescue-linux-disk-partitions/

Quy trình lắp mới một thiết bị lưu trữ gồm những bước sau:

- Xác định chuẩn lưu thông tin về partition (MBR, GPT).
- Gắn một ổ cứng mới vào server.
- Chia partition cho ổ cứng.
- Khởi tạo file system (ext4, xfs…) cho partition vừa mới được tạo.
- Mount partition lên hệ thống.
- Cấu hình auto mount partition khi reboot.

https://www.digitalocean.com/community/tutorials/how-to-partition-and-format-storage-devices-in-linux

Chọn tiêu chuẩn phân vùng (GPT hoặc MBR)

    parted /dev/vdb mklabel gpt
    parted /dev/vdb mklabel msdos

Tạo một primary partition mới với toàn bộ dung lượng của ổ `/dev/vdb`

    parted -a opt /dev/vdb mkpart primary ext4 0% 100%

Tạo filesystem ext4 cho phân vùng mới tạo

    mkfs.ext4 -L datapartition /dev/vdb1

![Imgur](https://i.imgur.com/MmcjbxY.png)

Tạo mount point và mount phân vùng vừa tạo được lên đó để sử dụng

    mkdir -p /mnt/data
    mount /dev/vdb1 /mnt/data/
    mount | grep /dev/vdb1

![Imgur](https://i.imgur.com/XuDApXS.png)

Kiểm tra lại phân vùng vừa khởi tạo

    lsblk --fs

![Imgur](https://i.imgur.com/o1nuqiP.png)

Tuy nhiên, với cách mount này, mỗi khi reboot, hệ thống sẽ không tự động mount lại phân vùng đó. Để cấu hình automount cho các phân vùng này, ta cần thiết lập trong file `/etc/fstab`

Mặc định, file `/etc/fstab` có nội dung:

![Imgur](https://i.imgur.com/qv2D7n9.png)

Mỗi dòng trong file này quy định cách thức mount các partition trên hệ thống và những option cần thiết để mount các partition đó. Mỗi dòng trong file tuân theo format sau:

    <Device> <Mount Point> <File System Type> <Options> <Dump> <Pass>

- `<device>`: Vị trí của thiết bị/phân vùng cần mount.
- `<mount point>`: Vị trí được mount lên.
- `<file system type>`: Loại filesystem (vfat, ntfs, ext2, ext3, ext4, jfs, xfs, swap, udf, iso9660, auto…)
- `<options>`: 
    - `default` gồm các option sau: rw, suid, dev, exec, auto, nouser, async
    - `sync/async`: sync – ghi xuống disk rồi mới chạy tiếp chương trình, async – “vờ như” đã ghi xuống đĩa (thật sự là ghi tạm lên buffer, lâu lâu mới ghi xuống đĩa 1 lần), chương trình có thể hoạt động tiếp mà không cần chờ ghi đĩa.
    - `auto/noauto`: tự động/không tự động mount khi boot hệ thống.
    - `dev/nodev`: dùng/không dùng special device (block, character) device
    - `exec/noexec`: cho phép/chặn không cho thực thi các file nhị phân trên filesystem.
    - `suid/nosuid`: cho phép/không cho phép dùng các bit SUID, SGID
    - `ro`: partition/thiết bị được mount với mode read-only.
    - `rw`: partition/thiết bị được mount với mode read-write.
    - `user`: cho phép user nào đó tương tác với partition được mount (mặc định là đi kèm với các option: noexec, nosuid, nodev).
    - `noouser`: chỉ cho phép user root tương tác với thiết bị được mount.
    - `_netdev`: xác định rằng đây là một thiết bị mạng, chỉ mount thiết bị này khi mạnh đã được start lên (dùng với filesystem nfs).
- `<dump>`: Bật/tắt tính năng backup filesystem. Tính năng này ít khi được dùng, giá trị mặc định là 0 (tắt), giá trị 1 là bật.
- `<pass num/fsck order>`: 

    Thông số này quyết định thứ tự mà lệnh fsck (filesytem check) thực thi với các partition được mount lúc boot hệ thống.

    - 0 == không check filesystem.
    - 1 == check partition này đầu tiên.
    - 2 == check partition này sau partition đầu tiên.

    Giá trị “1” được dùng cho root (/) partition, các partition còn lại sẽ được gán giá trị 2 (nếu cần check filesystem).

Ví dụ: Tự động mount partition sdb 1 lên thư mục /mnt/data mỗi khi reboot

Thêm dòng sau vào file `/etc/fstab`

    /dev/sdb1 /mnt/data ext4 defaults 0 0

Reboot và kiểm tra lại bằng lệnh lsblk

![Imgur](https://i.imgur.com/YwXmA25.png)

Partition /dev/sdb1 đã được tự động mount lên hệ thống khi boot

## `rclone`

https://www.tecmint.com/rclone-sync-files-from-cloud-storage/
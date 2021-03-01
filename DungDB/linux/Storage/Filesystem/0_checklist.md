# Tìm hiểu về storage - CEPH

I, Lý thuyết

1. Tìm hiểu về File system trong hệ điều hành Linux

+ Tổng quan về File system
+ Hệ thống tập tin trong Linux
+ Các loại filesystem trong Linux
+ Cấu trúc file system
+ FUSE - Filesystem in Userspace
+ Truy cập Filesystem
+ Cấu trúc, tổ chức thư mục
+ Quản lý quyền truy nhập
+ Cấu trúc File system
+ Thực thi File system
+ Cấp phát bộ nhớ system
+ Quản lý bộ nhớ trống system
+ Nâng cao hiệu năng
+ Khôi phục FS
+ IO Hệ thống IO, IO Hardware, Buffer and Cache

2. Tìm hiểu HDD - SSD

+ Tổng quan về hdd
+ Tổng quan về Sdd
+ Ram tĩnh và động
+ Phân loại ssd
+ Bộ nhớ flash
+ Tổng quan DAS NAS ISCSI SAN
+ Chuẩn kết nổi ổ đĩa
+ Vấn đề về tốc độ Disk
+ Cấu trúc, phân vùng Disk

3. Tìm hiểu Raid

+ Tổng quan Raid
+ Phân loại Raid cứng và mềm
+ Partition table
+ Tổng quan về Partition table
+ Phân vùng GPT
+ Phân vùng MBR
+ Chuẩn BIOS
+ Chuẩn UEFI

4. LVM

+ Tổng quan về LVM
+ Thác tác cơ bản trên LVM
+ LVM Snapshot
+ Thin Provisioning Volume
+ Striped Logical Volume
+ Mirrored Logical Volume
+ Cache Logical Volume

5. CEPH

-  Lý thuyết về Ceph

+ Tổng quan về Ceph
+ Kiến trúc và thành phần Ceph
+ Nội tại Ceph
+ Các vấn đề cần cân nhắc khi triển khai Ceph
+ Các giải pháp lưu trữ của Ceph
+ Tổng hợp các câu lệnh thường sử dụng trên Ceph

- Các service trong CEPH

+ Ceph RADOS
+ CRUSH
+ Ceph Storage Backend
+ Ceph MON - Monitor (ceph-mon)
+ Ceph OSD - Object Storage Device (ceph-osd)
+ Ceph RBD - RADOS Block Device
+ Ceph MDS - Metadata Server (ceph-mds)
+ Ceph RADOSGW - Object Gateway(ceph-radosgw)
+ Ceph MGR - Manager (ceph-mgr)
+ Thuật toán PAXOS
+ Cơ chế xác thực của Ceph
+ Các flag của Cluster Ceph
+ Các trạng thái của PG

Lý thuyết tham khảo:
https://github.com/lacoski/khoa-luan
https://github.com/uncelvel/tutorial-ceph

II, Thực hành

1. Cài đặt CEPH phiên bản nautilus
2. Cài đặt CEPH phiên bản luminous
3. Bài lab kết nối ceph client - ceph cluster
4. Tập lệnh thao tác trên CEPH
5. Tích hợp CEPH - Openstack (Chưa lab)
6. Cài đặt CEPH phiên bản luminous user
7. Add/remove một host trong ceph cluster
8. Chuyển OSD của node lỗi sang node mới
9. Add thêm hoặc remove thêm node mon, mgr
10. Set flag trong CEPH Cluster trong bảo trì hệ thống
11. Quản trị service Ceph
12. Thao tác với câu lệnh kiểm tra trạng thái hệ thống
13. Các câu lệnh kiểm tra trạng thái OSD
14. Các câu lệnh quản trị pools
15. Các câu lệnh quản trị RBD Images
16. Câu lệnh quản trị Mon Map
17. Vị trí log Ceph
18. Thay đổi config Ceph
19. Chỉnh sửa CRUSH Map 
- Tách pool SSD và pool HDD
- Tách pool theo CRUSH TREE
20. Backup restore Ceph với RDB Export Import
21. Hướng dẫn migrate volumes giữa 2 Cluster Ceph
22. Hướng dẫn cài đặt benji-backup trên CentOS 7, Ubuntu18
23. Hướng dẫn cài đặt backy2 trên Ubuntu
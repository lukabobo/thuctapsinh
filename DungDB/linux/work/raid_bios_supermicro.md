# RAID trên BIOS đối với server supermicro

Đầu tiên vào BIOS setup. Chuyển SATA mode từ AHCI về RAID mode. Sau đó thoát, đợi máy reboot lại rồi nhấn Ctrl+I để vào RAID. Cài đặt RAID như bình thường đối với RAID 1, RAID 5,... Sau đó cài OS.

Đối với RAID 0 thì không cần làm các bước trên. Cài trực tiếp OS với mode AHCI.
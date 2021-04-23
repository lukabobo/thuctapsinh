# Mount thêm ổ vào server 

Mount thêm ổ sdb

    mkfs.ext4 /dev/sdb #định dạng ext4
    blkid # lấy UUID
    echo "UUID=3c1d8220-c090-4809-b0e3-922f52e44186 /home ext4 defaults 0 0" >> /etc/fstab
    mount /dev/sdb /home/
    reboot

    45a81c28-8b57-4914-b8ca-cc64b97e6e53
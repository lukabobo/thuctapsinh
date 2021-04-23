## Script check disk và gửi mail 

https://serverok.in/bash-script-to-monitor-disk-usage

## Kiểm tra tốc độ disk

https://www.shellhacks.com/disk-speed-test-read-write-hdd-ssd-perfomance-linux/

On Linux Mint, Ubuntu, Debian:

    apt-get install hdparm -y

On CentOS, RHEL:

    yum install hdparm -y

Run hdparm as follows, to measure the READ speed of a storage drive device /dev/sda:

    hdparm -T /dev/sda
    hdparm -T /dev/sdb

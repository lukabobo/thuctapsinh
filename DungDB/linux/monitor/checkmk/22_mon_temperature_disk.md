# Giám sát nhiệt độ disk

Nhiệt độ bình thường của các thiết bị:

- CPU: 60, 70 độ C
- HDD: 40, 50 độ C
- Các thiết bị khác: 70, 80 độ C

Ở đây chúng ta giám sát nhiệt độ disk bằng plugin S.M.A.R.T

Vào host cần giám sát disk chạy các lệnh sau:

    yum install smartmontools -y && sudo apt-get install smartmontools
    cd /usr/lib/check_mk_agent/plugins
    wget https://raw.githubusercontent.com/uncelvel/tutorial-ceph/master/docs/monitor/check_mk/plugins/smart
    chmod +x smart
    ./smart

Sau đó vào giao diện web checkmk discovery host vừa cài plugin và active change.

Kết quả:

![Imgur](https://i.imgur.com/hJYtucg.png)
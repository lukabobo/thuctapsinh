# Giám sát bad disk 

Thực hiện trên host cần giám sát

    yum install smartmontools -y && sudo apt-get install smartmontools

    cd /usr/lib/check_mk_agent/plugins

    wget https://raw.githubusercontent.com/uncelvel/tutorial-ceph/master/docs/monitor/check_mk/plugins/smart

    chmod +x smart

    ./smart

Sau đó, chạy discovery lại là xong.
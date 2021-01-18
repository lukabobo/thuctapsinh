# Tích hợp grafana

Để cài đặt Grafana trên CentOS7 ta làm theo các bước sau:

Tìm phiên bản phù hợp ở đây: https://grafana.com/grafana/download

Tải grafana

    wget https://dl.grafana.com/oss/release/grafana-6.4.4-1.x86_64.rpm

Giải nén

    sudo yum localinstall grafana-6.4.4-1.x86_64.rpm

Cài đặt

    sudo yum install grafana

Khởi động dịch vụ

    sudo /sbin/chkconfig --add grafana-server
    systemctl start grafana-server
    systemctl enable grafana-server
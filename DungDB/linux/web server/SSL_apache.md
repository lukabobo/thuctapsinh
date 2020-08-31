# Cài SSL cho apache

## Cài Mod SSL

    yum -y install httpd mod_ssl
    sudo systemctl enable httpd.service
    systemctl start httpd.service

## Cài chứng chỉ

Tạo các file chứng chỉ:

    /var/www/domain/zone/ssl/<your-domain>/public.crt
    /var/www/domain/zone/ssl/<your-domain>/ca.crt
    /var/www/domain/zone/ssl/<your-domain>/private.key

Phân quyền

    chmod 700 /var/www/domain/zone/ssl/

Setup virtual host

    vi /etc/httpd/conf.d/ssl.conf

```
<VirtualHost *:443>
        SSLEngine on
        SSLCertificateFile /var/www/domain/zone/ssl/<your-domain>/public.crt
        SSLCACertificateFile /var/www/domain/zone/ssl/<your-domain>/ca.crt
        SSLCertificateKeyFile /var/www/domain/zone/ssl/<your-domain>/private.key
        DocumentRoot "/var/www/domain/source/<your-domain>"
    ServerName <your-domain>
</VirtualHost>
```

## Redirect HTTPS

    vi /etc/httpd/conf/httpd.conf
```
<VirtualHost *:80>
        ServerName www.<your-domain>
        Redirect "/" "https://www.<your-domain>/"
</VirtualHost>
```

## Test cấu hình trước khi restart dịch vụ

    apachectl configtest

restart dịch vụ

    systemctl restart httpd

Sau khi cài vào trang https://www.sslshopper.com/ssl-checker.html để check

## Cấu hình mẫu:

```
vi /etc/httpd/conf.d/ssl.conf

<VirtualHost *:443>
        SSLProtocol all -SSLv2 -SSLv3 #phiên bản SSL
        SSLEngine on    #dùng SSLEngine
        SSLCertificateFile /var/www/domain/zone/ssl/plasticpmc.com/public.crt   #Vị trí đặt chứng chỉ (file cert)
        SSLCACertificateFile /var/www/domain/zone/ssl/plasticpmc.com/ca.crt #Vị trí đặt chứng chỉ (file ca)
        SSLCertificateKeyFile /var/www/domain/zone/ssl/plasticpmc.com/private.key   #Vị trí đặt chứng chỉ (file private key)

        # Cần phải có đủ 3 file trên

    ServerAdmin cpanel@nhanhoa.com
        DocumentRoot "/var/www/domain/source/plasticpmc.com"
                <Directory  "/var/www/domain/source/plasticpmc.com">    #vị trí đặt source code web
                        Options Indexes FollowSymLinks MultiViews   #cho phép truy cập vào thư mục không có DirectoryIndex, cho sử dụng symbolic link để tăng hiệu năng 
                        AllowOverride all   #Cho phép tệp .htaccess ghi đè mọi thông số cấu hình được đặt trong apache2.conf
                        Order allow,deny    #Theo mặc định, không cho phép truy cập vào một thư mục cụ thể
                            Allow from all  #Cho phép kết nối từ bất kỳ mạng nào.
                </Directory>
    ServerName plasticpmc.com   #tên domain
    ServerAlias www.plasticpmc.com  #alias
    ErrorLog logs/plasticpmc.com-error_log  #log lỗi gặp phải khi xử lý các yêu cầu sẽ được lưu ở đây
    CustomLog logs/plasticpmc.com-access_log common # CustomLog (sử dụng để ghi các yêu cầu tới máy chủ) lưu tại đây
</VirtualHost>
```

# Tham khảo 

https://www.loggly.com/ultimate-guide/apache-logging-basics/

https://medium.com/@hbayraktar/how-to-install-ssl-certificate-on-apache-for-centos-7-38c25b84d8b1

https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-centos-7
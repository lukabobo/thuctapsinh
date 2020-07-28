# Plugins

Xem danh sách các plugins tại đây: https://checkmk.com/cms_check_plugins_catalog.html#Linux

Tại GUI, tìm các plugin cần dùng tại Check plugins

![Imgur](https://i.imgur.com/okAXOn0.png)

![Imgur](https://i.imgur.com/O9ZAhqC.png)

Các plugin phổ biến có sẵn trên checkmk server đặt tại thư mục: `/opt/omd/versions/1.6.0p13.cre/share/check_mk/agents/plugins/`. Tùy vào phiên bản checkmk mà đường dẫn sẽ khác

Các plugin có sẵn trên Linux:

![Imgur](https://i.imgur.com/ejngkwt.png)

Cần phải phần quyền cho chúng trước khi scp sang các host để cài đặt

Ví dụ: 

    chown 755 /opt/omd/versions/1.6.0p13.cre/share/check_mk/agents/plugins/mk_mysql

    scp /opt/omd/versions/1.6.0p13.cre/share/check_mk/agents/plugins/mk_mysql root@10.10.10.115:/usr/lib/check_mk_agent/plugins

Sau khi truyền file cần phải cho nó quyền thực thi

    chmod +x /usr/lib/check_mk_agent/plugins/mk_mysql

Tùy vào hệ điều hành, hãng sản xuất, phần mềm cần giám sát,... mà ta dùng các plugin khác nhau. Mỗi plugin sẽ có cách cấu hình khác nhau.

Một số plugin cần có file cấu hình trong `/etc/check_mk/`. Một số không yêu cầu bắt buộc phải cấu hình. Một số khác có thể sử dụng ngay được luôn. 

Để sử dụng plugin có thể đọc các thông tin của nó ở `Check plugins`, đọc phần comment ngay trong plugins, tìm thông tin trên trang của hãng.
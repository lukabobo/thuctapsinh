# Giám sát apache

## Thực hiện giám sát trên Centos 7

Làm theo hướng dẫn ở đây:

https://github.com/niemdinhtrong/thuctapsinh/blob/master/NiemDT/Ghichep_checkmk/docs/06.Giam-sat-apache.md

Copy file plugin từ checkmk server `/omd/versions/1.6.0p13.cre/share/check_mk/agents/plugins/apache_status` sang thư mục `/usr/lib/check_mk_agent/plugins` trên agent.

    scp /omd/versions/1.6.0p13.cre/share/check_mk/agents/plugins/apache_status root@10.10.10.115:/usr/lib/check_mk_agent/plugins

Lệnh kiểm tra:

    check_mk_agent | grep -A 3 apache_status

Kết quả:

![Imgur](https://i.imgur.com/WwRzRXy.png)

## Thực hiện trên Ubuntu 18

Thực hiện trên Ubuntu tương tự. Chỉ khác ở chỗ sửa file `/etc/apache2/apache2.conf` thay vì `/etc/httpd/conf/httpd.conf` như trên Centos.

Nội dung

```
<IfModule mod_status.c>
<Location /server-status>
SetHandler server-status
Order deny,allow
Deny from all
Allow from 127.0.0.1 ::1
</Location>
# Keep track of extended status information for each request
ExtendedStatus On
</IfModule>
```

Sửa file `/etc/apache2/mods-enabled/status.conf`

Sửa dòng `#Require ip 127.0.0.1/8` thành `Require ip 127.0.0.1/8`

![Imgur](https://i.imgur.com/ABhcr0h.png)

Thực hiện restart lại apache2 

    systemctl restart apache2

Kiểm tra:

    curl http://127.0.0.1/server-status

Kết quả đúng như sau:

```
root@hostu18test:~# curl http://127.0.0.1/server-status
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html><head>
<title>Apache Status</title>
</head><body>
<h1>Apache Server Status for 127.0.0.1 (via 127.0.0.1)</h1>

<dl><dt>Server Version: Apache/2.4.29 (Ubuntu)</dt>
<dt>Server MPM: prefork</dt>
<dt>Server Built: 2020-03-13T12:26:16
</dt></dl><hr /><dl>
<dt>Current Time: Monday, 27-Jul-2020 02:42:22 UTC</dt>
<dt>Restart Time: Monday, 27-Jul-2020 02:30:22 UTC</dt>
<dt>Parent Server Config. Generation: 1</dt>
<dt>Parent Server MPM Generation: 0</dt>
<dt>Server uptime:  12 minutes</dt>
<dt>Server load: 0.00 0.02 0.00</dt>
<dt>Total accesses: 20 - Total Traffic: 32 kB</dt>
<dt>CPU Usage: u.02 s.01 cu0 cs0 - .00417% CPU load</dt>
<dt>.0278 requests/sec - 45 B/second - 1638 B/request</dt>
<dt>1 requests currently being processed, 5 idle workers</dt>
</dl><pre>W_____..........................................................
................................................................
......................</pre>
<p>Scoreboard Key:<br />
"<b><code>_</code></b>" Waiting for Connection,
"<b><code>S</code></b>" Starting up,
"<b><code>R</code></b>" Reading Request,<br />
"<b><code>W</code></b>" Sending Reply,
...
```

Cài một số gói cần thiết

    yum install net-tools

Copy file plugin từ checkmk server `/omd/versions/1.6.0p13.cre/share/check_mk/agents/plugins/apache_status` sang thư mục `/usr/lib/check_mk_agent/plugins` trên agent.

    scp /omd/versions/1.6.0p13.cre/share/check_mk/agents/plugins/apache_status root@10.10.10.115:/usr/lib/check_mk_agent/plugins

**Cài python** (Plugin được viết bằng python. Trên Centos đã có sẵn python nên không cần cài. Ubuntu không có sẵn python)

    apt -y install python

Cấp quyền thực thi cho file

chmod +x /usr/lib/check_mk_agent/plugins/apache_status

Lệnh kiểm tra:

    check_mk_agent | grep -A 3 apache_status

![Imgur](https://i.imgur.com/3x8xNnF.png)

Kết quả có cách dòng như ở dưới tức là đã thực hiện đúng

```
<<<apache_status:sep(124)>>>
[::1]|80||::1
[::1]|80||ServerVersion: Apache/2.4.29 (Ubuntu)
[::1]|80||ServerMPM: prefork
```

Sau đó trên WATO ta thực hiện Discovery đối với thư mục chứa host này (hoặc chỉ đối với host này)

Kết quả:

![Imgur](https://i.imgur.com/PibhQdU.png)
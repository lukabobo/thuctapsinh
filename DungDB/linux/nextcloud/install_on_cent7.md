# Note lại cách cài đặt Nextcloud trên Centos 7

Trước hết cần tắt SELinux. Firewall cần mở port 80 và 443.

## Cài đặt Nextcloud

### Cài đặt http
```
yum update -y && yum install epel-release -y
yum install vim wget mlocate telnet net-tools bind-utils yum-utils -y

sudo yum -y install httpd

systemctl start httpd.service
systemctl enable httpd.service
```

### Cài đặt mariadb
```
sudo yum install -y mariadb-server

systemctl start mariadb
systemctl enable mariadb.service

mysql_secure_installation

mysql -uroot -p

CREATE DATABASE nextcloud;
CREATE USER 'nc_user'@'localhost' IDENTIFIED BY 'YOUR_PASSWORD_HERE';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nc_user'@'localhost';
FLUSH PRIVILEGES;
exit;
```

### Cài đặt php
```
sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum -y install https://rpms.remirepo.net/enterprise/remi-release-7.rpm

yum-config-manager --enable remi-php74

yum install php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysql php-zip php-dom php-xml php-mbstring php-libxml -y

php -v
```

Download và cài đặt Nextcloud
```
wget https://download.nextcloud.com/server/releases/nextcloud-21.0.0.zip -O /opt/nextcloud.zip

yum install unzip -y
unzip /opt/nextcloud.zip -d /var/www/

sudo chmod 755 -R /var/www/nextcloud/
sudo chown apache. -R /var/www/nextcloud/
```
```
cat << EOF >> /etc/httpd/conf.d/nextcloud.conf
<VirtualHost *:80>
    ServerAdmin admin@yourdomain.com
    DocumentRoot /var/www/nextcloud
    <Directory /var/www/html/nextcloud>
    Options +FollowSymlinks
    AllowOverride All
    <IfModule mod_dav.c>
    Dav off
    </IfModule>
    SetEnv HOME /var/www/nextcloud
    SetEnv HTTP_HOME /var/www/nextcloud
    </Directory>
    ErrorLog /var/log/httpd/nextcloud-error_log
    CustomLog /var/log/httpd/nextcloud-access_log common
</VirtualHost>
EOF
```
    systemctl restart httpd

Truy cập vào giao diện web chọn DB mysql và finish setup

## Cài đặt Collabora

### Cài đặt docker
```
yum install -y yum-utils device-mapper-persistent-data lvm2

yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

yum install -y docker-ce docker-ce-cli containerd.io

systemctl start docker
systemctl enable docker
```

Download image collabora

```
docker pull collabora/code
docker run -t -d -p 127.0.0.1:9980:9980 --restart always --cap-add MKNOD collabora/code
```
Thông thường tích hợp collabora sẽ require SSL và domain để không require ta thực hiện như sau. Cơ bản sẽ set enable SSL = false và termination ssl = false 

    cd /root/

```
cat << EOF > loolwsd.xml
<config>
    <allowed_languages desc="List of supported languages of Writing Aids (spell checker, grammar checker, thesaurus, hyphenation) on this instance. Allowing too many has negative effect on startup performance." default="de_DE en_GB en_US es_ES fr_FR it nl pt_BR pt_PT ru">de_DE en_GB en_US es_ES fr_FR it nl pt_BR pt_PT ru</allowed_languages>

    <sys_template_path desc="Path to a template tree with shared libraries etc to be used as source for chroot jails for child processes." type="path" relative="true" default="systemplate"></sys_template_path>
    <child_root_path desc="Path to the directory under which the chroot jails for the child processes will be created. Should be on the same file system as systemplate and lotemplate. Must be an empty directory." type="path" relative="true" default="jails"></child_root_path>
    <mount_jail_tree desc="Controls whether the systemplate and lotemplate contents are mounted or not, which is much faster than the default of linking/copying each file." type="bool" default="true"></mount_jail_tree>

    <server_name desc="External hostname:port of the server running loolwsd. If empty, it's derived from the request (please set it if this doesn't work). Must be specified when behind a reverse-proxy or when the hostname is not reachable directly." type="string" default=""></server_name>
    <file_server_root_path desc="Path to the directory that should be considered root for the file server. This should be the directory containing loleaflet." type="path" relative="true" default="loleaflet/../"></file_server_root_path>

    <memproportion desc="The maximum percentage of system memory consumed by all of the Collabora Online Development Edition, after which we start cleaning up idle documents" type="double" default="80.0"></memproportion>
    <num_prespawn_children desc="Number of child processes to keep started in advance and waiting for new clients." type="uint" default="1">1</num_prespawn_children>
    <per_document desc="Document-specific settings, including LO Core settings.">
        <max_concurrency desc="The maximum number of threads to use while processing a document." type="uint" default="4">4</max_concurrency>
        <batch_priority desc="A (lower) priority for use by batch eg. convert-to processes to avoid starving interactive ones" type="uint" default="5">5</batch_priority>
        <document_signing_url desc="The endpoint URL of signing server, if empty the document signing is disabled" type="string" default=""></document_signing_url>
        <redlining_as_comments desc="If true show red-lines as comments" type="bool" default="false">false</redlining_as_comments>
        <idle_timeout_secs desc="The maximum number of seconds before unloading an idle document. Defaults to 1 hour." type="uint" default="3600">3600</idle_timeout_secs>
        <idlesave_duration_secs desc="The number of idle seconds after which document, if modified, should be saved. Defaults to 30 seconds." type="int" default="30">30</idlesave_duration_secs>
        <autosave_duration_secs desc="The number of seconds after which document, if modified, should be saved. Defaults to 5 minutes." type="int" default="300">300</autosave_duration_secs>
        <always_save_on_exit desc="On exiting the last editor, always perform the save, even if the document is not modified." type="bool" default="false">false</always_save_on_exit>
        <limit_virt_mem_mb desc="The maximum virtual memory allowed to each document process. 0 for unlimited." type="uint">0</limit_virt_mem_mb>
        <limit_stack_mem_kb desc="The maximum stack size allowed to each document process. 0 for unlimited." type="uint">8000</limit_stack_mem_kb>
        <limit_file_size_mb desc="The maximum file size allowed to each document process to write. 0 for unlimited." type="uint">0</limit_file_size_mb>
        <limit_num_open_files desc="The maximum number of files allowed to each document process to open. 0 for unlimited." type="uint">0</limit_num_open_files>
        <limit_load_secs desc="Maximum number of seconds to wait for a document load to succeed. 0 for unlimited." type="uint" default="100">100</limit_load_secs>
        <limit_convert_secs desc="Maximum number of seconds to wait for a document conversion to succeed. 0 for unlimited." type="uint" default="100">100</limit_convert_secs>
        <cleanup desc="Checks for resource consuming (bad) documents and kills associated kit process. A document is considered resource consuming (bad) if is in idle state for idle_time_secs period and memory usage passed limit_dirty_mem_mb or CPU usage passed limit_cpu_per" enable="false">
            <cleanup_interval_ms desc="Interval between two checks" type="uint" default="10000">10000</cleanup_interval_ms>
            <bad_behavior_period_secs desc="Minimum time period for a document to be in bad state before associated kit process is killed. If in this period the condition for bad document is not met once then this period is reset" type="uint" default="60">60</bad_behavior_period_secs>
            <idle_time_secs desc="Minimum idle time for a document to be candidate for bad state" type="uint" default="300">300</idle_time_secs>
            <limit_dirty_mem_mb desc="Minimum memory usage for a document to be candidate for bad state" type="uint" default="3072">3072</limit_dirty_mem_mb>
            <limit_cpu_per desc="Minimum CPU usage for a document to be candidate for bad state" type="uint" default="85">85</limit_cpu_per>
        </cleanup>
    </per_document>

    <per_view desc="View-specific settings.">
        <out_of_focus_timeout_secs desc="The maximum number of seconds before dimming and stopping updates when the browser tab is no longer in focus. Defaults to 120 seconds." type="uint" default="120">120</out_of_focus_timeout_secs>
        <idle_timeout_secs desc="The maximum number of seconds before dimming and stopping updates when the user is no longer active (even if the browser is in focus). Defaults to 15 minutes." type="uint" default="900">900</idle_timeout_secs>
    </per_view>

    <loleaflet_html desc="Allows UI customization by replacing the single endpoint of loleaflet.html" type="string" default="loleaflet.html">loleaflet.html</loleaflet_html>

    <logging>
        <color type="bool">true</color>
        <level type="string" desc="Can be 0-8, or none (turns off logging), fatal, critical, error, warning, notice, information, debug, trace" default="warning">warning</level>
        <protocol type="bool" desc="Enable minimal client-site JS protocol logging from the start">false</protocol>
        <lokit_sal_log type="string" desc="Fine tune log messages from LOKit. Default is to suppress log messages from LOKit." default="-INFO-WARN">-INFO-WARN</lokit_sal_log>
        <file enable="false">
            <property name="path" desc="Log file path.">/var/log/loolwsd.log</property>
            <property name="rotation" desc="Log file rotation strategy. See Poco FileChannel.">never</property>
            <property name="archive" desc="Append either timestamp or number to the archived log filename.">timestamp</property>
            <property name="compress" desc="Enable/disable log file compression.">true</property>
            <property name="purgeAge" desc="The maximum age of log files to preserve. See Poco FileChannel.">10 days</property>
            <property name="purgeCount" desc="The maximum number of log archives to preserve. Use 'none' to disable purging. See Poco FileChannel.">10</property>
            <property name="rotateOnOpen" desc="Enable/disable log file rotation on opening.">true</property>
            <property name="flush" desc="Enable/disable flushing after logging each line. May harm performance. Note that without flushing after each line, the log lines from the different processes will not appear in chronological order.">false</property>
        </file>
        <anonymize>
            <anonymize_user_data type="bool" desc="Enable to anonymize/obfuscate of user-data in logs. If default is true, it was forced at compile-time and cannot be disabled." default="false">false</anonymize_user_data>
            <anonymization_salt type="uint" desc="The salt used to anonymize/obfuscate user-data in logs. Use a secret 64-bit random number." default="82589933">82589933</anonymization_salt>
        </anonymize>
    </logging>

    <loleaflet_logging desc="Logging in the browser console" default="false">false</loleaflet_logging>

    <trace desc="Dump commands and notifications for replay. When 'snapshot' is true, the source file is copied to the path first." enable="false">
        <path desc="Output path to hold trace file and docs. Use '%' for timestamp to avoid overwriting. For example: /some/path/to/looltrace-%.gz" compress="true" snapshot="false"></path>
        <filter>
            <message desc="Regex pattern of messages to exclude"></message>
        </filter>
        <outgoing>
            <record desc="Whether or not to record outgoing messages" default="false">false</record>
        </outgoing>
    </trace>

    <net desc="Network settings">
      <proto type="string" default="all" desc="Protocol to use IPv4, IPv6 or all for both">all</proto>
      <listen type="string" default="any" desc="Listen address that loolwsd binds to. Can be 'any' or 'loopback'.">any</listen>
      <service_root type="path" default="" desc="Prefix all the pages, websockets, etc. with this path."></service_root>
      <proxy_prefix type="bool" default="false" desc="Enable a ProxyPrefix to be passed int through which to redirect requests"></proxy_prefix>
      <post_allow desc="Allow/deny client IP address for POST(REST)." allow="true">
        <host desc="The IPv4 private 192.168 block as plain IPv4 dotted decimal addresses.">192\.168\.[0-9]{1,3}\.[0-9]{1,3}</host>
        <host desc="Ditto, but as IPv4-mapped IPv6 addresses">::ffff:192\.168\.[0-9]{1,3}\.[0-9]{1,3}</host>
        <host desc="The IPv4 loopback (localhost) address.">127\.0\.0\.1</host>
        <host desc="Ditto, but as IPv4-mapped IPv6 address">::ffff:127\.0\.0\.1</host>
        <host desc="The IPv6 loopback (localhost) address.">::1</host>
        <host desc="The IPv4 private 172.17.0.0/16 subnet (Docker).">172\.17\.[0-9]{1,3}\.[0-9]{1,3}</host>
        <host desc="Ditto, but as IPv4-mapped IPv6 addresses">::ffff:172\.17\.[0-9]{1,3}\.[0-9]{1,3}</host>
      </post_allow>
      <frame_ancestors desc="Specify who is allowed to embed the LO Online iframe (loolwsd and WOPI host are always allowed). Separate multiple hosts by space."></frame_ancestors>
      <connection_timeout_secs desc="Specifies the connection, send, recv timeout in seconds for connections initiated by loolwsd (such as WOPI connections)." type="int" default="30"></connection_timeout_secs>
    </net>

    <ssl desc="SSL settings">
        <enable type="bool" desc="Controls whether SSL encryption between browser and loolwsd is enabled (do not disable for production deployment). If default is false, must first be compiled with SSL support to enable." default="true">false</enable>
        <termination desc="Connection via proxy where loolwsd acts as working via https, but actually uses http." type="bool" default="true">false</termination>
        <cert_file_path desc="Path to the cert file" relative="false">/etc/loolwsd/cert.pem</cert_file_path>
        <key_file_path desc="Path to the key file" relative="false">/etc/loolwsd/key.pem</key_file_path>
        <ca_file_path desc="Path to the ca file" relative="false">/etc/loolwsd/ca-chain.cert.pem</ca_file_path>
        <cipher_list desc="List of OpenSSL ciphers to accept" default="ALL:!ADH:!LOW:!EXP:!MD5:@STRENGTH"></cipher_list>
        <hpkp desc="Enable HTTP Public key pinning" enable="false" report_only="false">
            <max_age desc="HPKP's max-age directive - time in seconds browser should remember the pins" enable="true">1000</max_age>
            <report_uri desc="HPKP's report-uri directive - pin validation failure are reported at this URL" enable="false"></report_uri>
            <pins desc="Base64 encoded SPKI fingerprints of keys to be pinned">
            <pin></pin>
            </pins>
        </hpkp>
    </ssl>

    <security desc="Altering these defaults potentially opens you to significant risk">
      <seccomp desc="Should we use the seccomp system call filtering." type="bool" default="true">true</seccomp>
      <capabilities desc="Should we require capabilities to isolate processes into chroot jails" type="bool" default="true">true</capabilities>
    </security>

    <watermark>
      <opacity desc="Opacity of on-screen watermark from 0.0 to 1.0" type="double" default="0.2"></opacity>
      <text desc="Watermark text to be displayed on the document if entered" type="string"></text>
    </watermark>

    <welcome>
      <enable type="bool" desc="Controls whether the welcome screen should be shown to the users on new install and updates." default="true">true</enable>
      <enable_button type="bool" desc="Controls whether the welcome screen should have an explanatory button instead of an X button to close the dialog." default="false">false</enable_button>
      <path desc="Path to 'welcome-$lang.html' files served on first start or when the version changes. When empty, defaults to the Release notes." type="path" relative="true" default="loleaflet/welcome"></path>
    </welcome>

    <user_interface>
      <mode type="string" desc="Controls the user interface style (classic|notebookbar)" default="classic">classic</mode>
    </user_interface>

    <storage desc="Backend storage">
        <filesystem allow="false" />
        <wopi desc="Allow/deny wopi storage. Mutually exclusive with webdav." allow="true">
            <host desc="Regex pattern of hostname to allow or deny." allow="true">localhost</host>
            <host desc="Regex pattern of hostname to allow or deny." allow="true">10\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}</host>
            <host desc="Regex pattern of hostname to allow or deny." allow="true">172\.1[6789]\.[0-9]{1,3}\.[0-9]{1,3}</host>
            <host desc="Regex pattern of hostname to allow or deny." allow="true">172\.2[0-9]\.[0-9]{1,3}\.[0-9]{1,3}</host>
            <host desc="Regex pattern of hostname to allow or deny." allow="true">172\.3[01]\.[0-9]{1,3}\.[0-9]{1,3}</host>
            <host desc="Regex pattern of hostname to allow or deny." allow="true">192\.168\.[0-9]{1,3}\.[0-9]{1,3}</host>
            <host desc="Regex pattern of hostname to allow or deny." allow="false">192\.168\.1\.1</host>
            <max_file_size desc="Maximum document size in bytes to load. 0 for unlimited." type="uint">0</max_file_size>
            <reuse_cookies desc="When enabled, cookies from the browser will be captured and set on WOPI requests." type="bool" default="false">false</reuse_cookies>
            <locking desc="Locking settings">
                <refresh desc="How frequently we should re-acquire a lock with the storage server, in seconds (default 15 mins) or 0 for no refresh" type="int" default="900">900</refresh>
            </locking>
        </wopi>
        <webdav desc="Allow/deny webdav storage. Mutually exclusive with wopi." allow="false">
            <host desc="Hostname to allow" allow="false">localhost</host>
        </webdav>
        <ssl desc="SSL settings">
            <as_scheme type="bool" default="true" desc="When set we exclusively use the WOPI URI's scheme to enable SSL for storage">true</as_scheme>
            <enable type="bool" desc="If as_scheme is false or not set, this can be set to force SSL encryption between storage and loolwsd. When empty this defaults to following the ssl.enable setting"></enable>
            <cert_file_path desc="Path to the cert file" relative="false"></cert_file_path>
            <key_file_path desc="Path to the key file" relative="false"></key_file_path>
            <ca_file_path desc="Path to the ca file. If this is not empty, then SSL verification will be strict, otherwise cert of storage (WOPI-like host) will not be verified." relative="false"></ca_file_path>
            <cipher_list desc="List of OpenSSL ciphers to accept. If empty the defaults are used. These can be overriden only if absolutely needed."></cipher_list>
        </ssl>
    </storage>

    <tile_cache_persistent desc="Should the tiles persist between two editing sessions of the given document?" type="bool" default="true">true</tile_cache_persistent>

    <admin_console desc="Web admin console settings.">
        <enable desc="Enable the admin console functionality" type="bool" default="true">true</enable>
        <enable_pam desc="Enable admin user authentication with PAM" type="bool" default="false">false</enable_pam>
        <username desc="The username of the admin console. Ignored if PAM is enabled."></username>
        <password desc="The password of the admin console. Deprecated on most platforms. Instead, use PAM or loolconfig to set up a secure password."></password>
    </admin_console>

    <monitors desc="Addresses of servers we connect to on start for monitoring">
    </monitors>

</config>  

EOF
```
    docker cp loolwsd.xml 2db21f32ef1c:/etc/loolwsd/loolwsd.xml

Đợi vài giây cho container tự restart sau đó ta kiểm tra bằng lệnh
```
[root@localhost opt]# curl http://127.0.0.1:9980
OK
```
### Cài đặt nginx làm reverse proxy cho collobora

    yum install nginx -y 

Cài chung server với nextcloud nên cần sửa port để tránh conflict

    sed -i 's|80 default_server;|8080 default_server;|g' /etc/nginx/nginx.conf

Tạo virtualhost làm proxy

```
cat << EOF > /etc/nginx/conf.d/proxy.conf
server {
    listen       8081;
    #server_name  collabora.example.com;

    # static files
    location ^~ /loleaflet {
        proxy_pass http://localhost:9980;
        proxy_set_header Host \$http_host;
    }

    # WOPI discovery URL
    location ^~ /hosting/discovery {
        proxy_pass http://localhost:9980;
        proxy_set_header Host \$http_host;
    }

    # Capabilities
    location ^~ /hosting/capabilities {
        proxy_pass http://localhost:9980;
        proxy_set_header Host \$http_host;
    }

    # main websocket
    location ~ ^/lool/(.*)/ws$ {
        proxy_pass http://localhost:9980;
        proxy_set_header Upgrade \$http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host \$http_host;
        proxy_read_timeout 36000s;
    }

    # download, presentation and image upload
    location ~ ^/lool {
        proxy_pass http://localhost:9980;
        proxy_set_header Host \$http_host;
    }

    # Admin Console websocket
    location ^~ /lool/adminws {
        proxy_pass http://localhost:9980;
        proxy_set_header Upgrade \$http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host \$http_host;
        proxy_read_timeout 36000s;
    }
}
EOF
```
Kiểm tra và restart dịch vụ
```
nginx -t 
systemctl restart nginx
```

### Tích hợp Collabora với Nextcloud

Setting -> Collabora Online -> Use your own server và điền vào http://collabora.example.com và bấm lưu. 

Mở thử 1 file docx hoặc excel để test.
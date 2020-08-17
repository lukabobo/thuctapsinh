# Check dung lượng disk đang dùng và gửi mail cảnh báo nếu sắp

Nguồn: https://serverok.in/bash-script-to-monitor-disk-usage

Đầu tiên cài postfix để gửi mail. Hướng dẫn cài: https://github.com/lukabobo/thuctapsinh/blob/master/DungDB/linux/mail%20(postfix%20and%20ssmtp)/postfix.md

Nội dung script 

```
#!/bin/bash

CURRENT_USAGE=$(df / | grep -v 'Filesystem' | awk '{print $5}' | sed 's/%//g')
ALERT_ON=80

if [ "$CURRENT_USAGE" -gt "$ALERT_ON" ] ; then
    mail -s 'Disk Usage Warning' YOUR_EMAIL_HERE << EOF
Disk almost full on / partition. Current Useage: $CURRENT_USAGE%
EOF
fi
```

Sửa `YOUR_EMAIL_HERE` thành email ta sử dụng.

Nếu dung lượng đang sử dụng đạt 80% sẽ có cảnh báo về email. 

Test cảnh báo, tôi giảm ngưỡng cảnh báo xuống còn 10%.

Nội dung cảnh báo gửi về email như sau:

![Imgur](https://i.imgur.com/NmmrXzt.png)
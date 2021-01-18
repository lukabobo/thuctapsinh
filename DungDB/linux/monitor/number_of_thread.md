# Number of thread trên Linux

Thư mục chứa process: `/proc/`

Thư mục chứa các thread: `/proc/<PID>/task/`

Lấy số lượng thread từ process id

    ps -o nlwp <pid>

Lấy toàn bộ số lượng thread đang chạy của hệ thống

    ps -eo nlwp | tail -n +2 | awk '{ num_threads += $1 } END { print num_threads }'

Để kiểm tra số lượng thread được cho phép trên hệ thống ta dùng lệnh:

    cat /proc/sys/kernel/threads-max

![Imgur](https://i.imgur.com/XrMTJVr.png)

Số lượng tối đa này được tùy thuộc vào cấu hình phần cứng của server (server có bao nhiêu CPU, CPU có bao nhiêu core)

## Tham khảo:

https://www.golinuxcloud.com/check-threads-per-process-count-processes/

https://techmaster.vn/posts/33604/su-khac-nhau-giua-process-va-thread
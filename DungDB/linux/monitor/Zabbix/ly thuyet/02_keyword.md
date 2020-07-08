# Các thuật ngữ thường gặp khi làm việc với Zabbix

- host: là một thiết bị mạng mà ta muốn giám sát sử dụng địa chỉ IP/DNS

- host group: là một nhóm các host, nó có thể bao gồm các host và các template. Host group được sử dụng khi ta gán quyên truy cập host cho các nhóm người dùng khác nhau.

- item: các thông số mà bạn muốn thu thập từ các host để giám sát

- application: một nhóm các items trong một logical group

- value preprocessing: các dữ liệu được các host gửi về zabbix server sẽ được xử lý trước khi lưu và database

- trigger: là ngưỡng mà ta đặt ra để xuất hiện cảnh báo. VD như ta đặt một trigger là số RAM free < 200M sẽ xuất hiện cảnh báo.

- event: là sự thay đổi của một cái gì đó đáng chú ý ví dụ như trạng thái của trigger có sự thay đổi.

- event tag

- event corelation

- problem: một trạng thái của trigger

- problem update: các tùy chọn quản lý sự cố do zabbix cung cấp như thêm comment,...

- action: một phương tiện xác định trước để phản ứng với một event. VD như một hành động gửi thông báo và các điều kiện của nó khi một event xảy ra.

- escalation: một kịch bản các hoạt động bên trong một hành động.

- media: cách thức gửi thông báo vd như qua mail hoặc telegram

- notification: một thông báo về một số event gửi tới người quản trị thông qua các kênh thông báo

- remote command: các lệnh được xác định trước và được thực hiện trên các host mà ta giám sát

- template: những items, trigger, graphs, screens, application,... được tạo trong 1 template là có thể áp dụng nó với các host khác mà ko cần tạo lại

- web scenario: một hoặc một số requests HTTP dùng để check hoạt động của môt web site

- frontend: giao diện web được cung cấp bởi zabbix

- dashboard: một mục trên giao diện web hiển thị tóm tắt và trực quan một số thông tin quan trọng

- zabbix API

- Zabbix server: một quy trình trung tâm của phần mềm zabbix, tương tác với zabbix proxy và zabbix agent, tính toán các trigger và gửi đi thông báo. Là nơi xử ly trung tâm

- Zabbix agent: là một process được cài đặt trên host có nhiệm vụ thu thập dữ liệu.

- Zabbix proxy: một process thay mặt cho zabbix server thu thập dữ liệu và xử lý nó. Nó giúp giảm tải cho zabbix server

- netwwork discovery

- item prototype

- trigger prototype
# Các keyword liên quan

https://blog.tinohost.com/message-transfer-agent-la-gi/

https://www.hostinger.vn/huong-dan/giai-thich-giao-thuc-pop3-smtp-imap-la-gi-va-port-cua-chung/

https://afreshcloud.com/sysadmin/mail-terminology-mta-mua-msa-mda-smtp-dkim-spf-dmarc

## POP3

POP3 (Post Office Protocol version 3) được sử dụng để kết nối tới server email và tải email xuống máy tính cá nhân thông qua một ứng dụng email như Outlook, Thunderbird, Windows Mail, Mac Mail, vâng vâng.

Thông thường, email client sẽ có tùy chọn bạn có muốn giữ mail trên server sau khi tải về hay không. Nếu bạn đang truy cập một tài khoản bằng nhiều thiết, chúng tôi khuyên là nên chọn giữ lại bản copy trên server nếu không thiết bị thứ 2 sẽ không thể tải mail về được vì nó đã bị xóa sau khi tải về trên thiết bị 1. Cũng đáng để lưu ý là POP3 là giao thức 1 chiều, có nghĩa là email được “kéo” từ email server xuống email client.

Mặc đình, port POP3 là:

Port **110** – port không mã hóa

Port **995** – SSL/TLS port, cũng có thể được gọi là POP3S

## IMAP 

IMAP (Internet Message Access Protocol), POP3 cũng đều được dùng để kéo emails về emails client, tuy nhiên khác biệt với POP3 là nó chỉ kéo email headers về, nội dung email vẫn còn trên server. Đây là kênh liên lạc 2 chiều, thay đổi trên mail client sẽ được chuyển lên server. Sau này, giao thức này trở nên phổ biến nhờ nhà cung cấp mail lớn nhất thế giới, Gmail, khuyên dùng thay vì POP3.

Port IMAP mặc định:

Port **143** – port không mã hóa

Port **993** – SSL/TLS port, cũng có thể được gọi là IMAPS

Mục đích sử dụng POP3 và IMAP là giống nhau, đều dùng để tải mail nhưng IMAP giữ nội dung trên server và POP3 tải toàn bộ mail về máy tính.

IMAP cung cấp truy cập theo ba chế độ khác nhau: offline (ngoại tuyến), online (trực tuyến) và disconnected (ngắt kết nối). Chúng truy cập vào chế độ offline IMAP giống như các thông điệp email được truyền đến khách hàng hay đọc, trả lời, làm các việc khác ở chế độ ngoại tuyến theo sự cài đặt của đơn vị khách hàng.

Truy cập chế độ online là chế độ IMAP truy cập mà người dùng đọc và làm việc với thông điệp email trong khi vẫn đang giữ kết nối với mail server (kết nối mở). Các thông điệp này vẫn nẳm ở mail server cho đến khi nào người dùng quyết định xóa nó đi. Chúng đều được gắn nhãn hiệu cho biết loại để “đọc” hay “trả lời”.

Ngoài ra, trong chế độ disconnected, IMAP còn cho phép người dùng lưu tạm thông điệp ở client server và làm việc với chúng, sau đó cập nhập trở lại vào mail server ở lần kết nối tiếp.

## SMTP 

SMTP (Simple Mail Transfer Protocol) là giao thức chuẩn TCP/IP được dùng để truyền tải thư điện tử (e-mail) trên mạng internet.

Nó thiết lập kênh kết nối giữa mail client và mail server, và thiết lập kênh liên lạc giữa mail server gửi và mail server nhận. Email sẽ được đẩy từ mail client lên mail server và từ mail server nó sẽ được server này gửi đi đến mail server nhận. Nhìn hình dưới bạn sẽ thấy cách hoạt động của việc gửi mail:

![Imgur](https://i.imgur.com/fRx4KEX.png)

Các port mặc định của SMTP:

Port **25** – port không mã hóa

Port **465/587** – SSL/TLS port, cũng có thể được gọi là SMTPS

## MTA – Message transfer agent

Đây là dịch vụ xử lý các tin nhắn trực tuyến: chuyển thư từ máy tính đến một nơi khác.

Các chương trình cung cấp dịch vụ MTA tiêu biểu là: Qmail, Sendmail, Postfix (Linux), Edge/Hub Tranpost của MS Exchange Server (Windows).

Message Transfer Agent thực hiện cả nhiệm vụ gửi tin và nhận thư trong giao thức truyền tin đơn giản.

Hiểu đơn giản, Message Transfer Agent làm nhiệm vụ nhận email từ máy khác và chuyển nó đến đích cần đến. Các MTA đóng vai trò là SMTP server.

### Một số thuật ngữ liên quan

**Mail Delivery Agent (MDA)**

MDA là chương trình có nhiệm vụ chính là lưu trữ email vào đĩa cứng. Ngoài ra, MDA có thể lọc, sắp xếp email…
VD: maildrop, procmail.

**Mail User Agent (MUA)**

MUA là các chương trình gửi và nhận mail được cài đặt trên máy người dùng. MUA giúp người dùng quản lý, soạn thảo, nhận và gửi mail một cách tiện lợi và nhanh chóng.

Các chương trình MUA tiêu biểu là: Outlook (Windows), Evolution (Linux), ThunderBird va Eudora

### Nguyên lí hoạt động của MTA

Thuật ngữ máy chủ thư , bộ trao đổi thư và máy chủ MX cũng có thể ám chỉ đến một máy tính đang thực hiện chức năng MTA. Các Hệ thống tên miền (DNS) liên kết một mail server với một tên miền với một bản ghi MX có chứa các tên miền của máy chủ (s) cung cấp dịch vụ MTA.

Để làm việc thì MTA cần một tác nhân chuyển thư nhận thư từ một MTA khác. MTA khác là một tác nhân gửi thư (MSA) hoặc một tác nhân người dùng thư (MUA). Chi tiết truyền được chỉ định bởi giao thức truyền thư đơn giản (SMTP).

![Imgur](https://i.imgur.com/EB4sBDK.png)

###  Quá trình chuyển tiếp email giữa 2 Message Transfer Agents

Khi gửi email, MTA gửi sẽ xử lý tất cả các vấn đề liên quan đến việc gửi thư cho đến khi email đã được MTA khác nhận hoặc từ chối. Ngay khi email được gửi đi, nó sẽ đi qua mạng Internet. Mỗi MTA trong mạng Internet khi nhận email đều sẽ tra cứu địa chỉ nhận từ DNS để xác định MTA tiếp theo là đâu.

Dù từ cùng một mail server gửi và nhận, mỗi email của MTA khác có thể đi các đường khác nhau. Lý do là các email chọn đường đi tùy vào tính khả dụng của MTA.

Dựa theo những thông tin vừa lấy được thông qua Internet , email sẽ được gửi đi để đến mail server của người nhận.

Trong quá trình gửi đi, email sẽ được kiểm tra spam và virus bởi firewall.. Nếu có virus, email sẽ được cách ly. Đồng thời, người gửi sẽ nhận được một thông báo. Nếu bị đánh dấu là spam, email sẽ bị xóa mà không cần có thông báo tới người gửi. Bộ lọc sẽ kiểm tra dựa trên một loạt các tiêu chí để đảm bảo phát hiện spam tốt nhất.

Khi email đến mail server của người nhận, người nhận phải đăng nhập vào tài khoản của mình. Song song đó, người nhận sử dụng một trong các giao thức POP3 hoặc IMAP để lấy mail về.

- Với POP3: toàn bộ email trên mail server sẽ được tải về máy cá nhân. Toàn bộ email đã tải về trên mail server sẽ bị xóa. Tuy nhiên các mail server đều có thêm lựa chọn giữ lại 1 bản sao chứ không xóa hẳn.

- Với IMAP: email sẽ vẫn được lưu trữ trên mail server. Tuy nhiên sẽ có bản ánh xạ trên máy cá nhân. Khi người nhận xem email nào thì email đó sẽ được tải về và lưu ở chế độ tạm thời trên máy cá nhân. Khi tắt mail client, bản tạm đó cũng bị xóa đi.

### Một thư được chuyển thế nào?

- Bước 1: Khi các email được gởi đến từ MUA, MTA có nhiệm vụ nhận diện người gởi và người nhận từ thông tin đóng gói trong phần header của thư. Sau đó, điền các thông tin cần thiết vào header.

- Bước 2: MTA chuyển thư cho MDA để chuyển đến hộp thư ngay tại MTA, hoặc chuyển cho Remote MTA.

- Bước 3: Một phần hay cả bức thư có thể phải viết lại tại các MTA trên đường đi. SMTP là ngôn ngữ của MTAs

Đối với người nhận được lưu trữ cục bộ, việc gửi email cuối cùng đến hộp thư người nhận là nhiệm vụ của tác nhân gửi thư (MDA). Với mục đích này, MTA chuyển thông báo tới thành phần dịch vụ xử lý tin nhắn của tác nhân gửi thư (MDA). Khi giao hàng cuối cùng, trường Đường dẫn trả lại được thêm vào phong bì để ghi lại đường dẫn trả lại .


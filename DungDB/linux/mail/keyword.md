# Các keyword liên quan

https://blog.tinohost.com/message-transfer-agent-la-gi/

https://www.hostinger.vn/huong-dan/giai-thich-giao-thuc-pop3-smtp-imap-la-gi-va-port-cua-chung/

https://afreshcloud.com/sysadmin/mail-terminology-mta-mua-msa-mda-smtp-dkim-spf-dmarc

https://wiki.matbao.net/mail-server-la-gi-moi-thong-tin-can-biet-khi-thue-mail-server/

https://blog.tinohost.com/ptr-record-la-gi/

https://kienthuc.pavietnam.vn/article/Email-Server/Huong-dan-Thu-thuat/DMARC-la-gi?-Tai-sao-DMARC-lai-quan-trong-doi-voi-Mail-Server?.html

https://wiki.matbao.net/dmarc-la-gi-huong-dan-cach-tao-dmarc-record-don-gian-nhat/

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

**MSA (Mail Submission Agent)**

Một chương trình máy chủ nhận thư từ MUA, kiểm tra bất kỳ lỗi nào và chuyển nó (với SMTP) đến MTA được lưu trữ trên cùng một máy chủ.

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

![Imgur](https://i.imgur.com/wlzUoJC.png)

- Bước 1: Khi các email được gởi đến từ MUA, MSA kiểm tra xem thư có lỗi gì không. Sau đó chuyển cho MTA. MTA có nhiệm vụ nhận diện người gởi và người nhận từ thông tin đóng gói trong phần header của thư. Sau đó, điền các thông tin cần thiết vào header.

- Bước 2: MTA chuyển thư cho MDA để chuyển đến hộp thư ngay tại MTA, hoặc chuyển cho Remote MTA.

- Bước 3: Một phần hay cả bức thư có thể phải viết lại tại các MTA trên đường đi. SMTP là ngôn ngữ của MTAs

Đối với người nhận được lưu trữ cục bộ, việc gửi email cuối cùng đến hộp thư người nhận là nhiệm vụ của tác nhân gửi thư (MDA). Với mục đích này, MTA chuyển thông báo tới thành phần dịch vụ xử lý tin nhắn của tác nhân gửi thư (MDA). Khi giao hàng cuối cùng, trường Đường dẫn trả lại được thêm vào phong bì để ghi lại đường dẫn trả lại.

Người nhận thư sẽ thông qua giao thức POP3 hoặc IMAP để lấy thư từ hộp thư về MUA của họ.

# Các bản ghi liên quan

Để mail server hoạt động, các thư gửi đi được tin tưởng thì cần có các bản ghi sau:

- Thêm bản ghi A: Tên mail loại bản A giá trị IP

- Thêm bản ghi MX: Tên @ loại bản ghi MX giá trị mail.domain

- Thêm bản ghi _dmarc: _dmarc loại bản ghi txt giá trị v=DMARC1; p=none; rua=mailto:mailauth-reports@mail.domain

- Thêm bản ghi SPF: @ loại bản ghi txt giá trị v=spf1 +a +mx +ip4:1<IP> ~all

- Thêm bản ghi PTR ở DNS server

- Thêm bản ghi DKIM các thông số lấy từ email server

## Bản ghi MX

Định nghĩa về MX Record:

MX Record là viết tắt của Mail Exchanger Record được định nghĩa là một bản ghi trong DNS zone dùng để định vị Mail Server cho một Domain.

Một tên miền có thể được gán bởi nhiều bản ghi MX, việc này giúp cho các email của bạn không bị mất đi dữ liệu nếu ngưng hoạt động một thời gian.

Ví dụ: Khi bạn gửi Email đến địa chỉ whitemail@example.com, Mail Server sẽ xác định xem MX Record example.com được điều khiển bởi Mail Server nào (chẳng hạn như mail.example.com), sau đó sẽ xem A Record để chuyển đến IP đích. Ngoài ra, MX Record còn tự động ghi thêm một giá trị ngoài tên miền của Mail Exchange một số thứ tự để tham chiếu. Số này có giá trị nguyên không dấu 16-bit. Đó được hiểu là thứ tự ưu tiên giữa các Mail Exchanger.

Bản ghi MX phải được sử dụng cùng với bản ghi A. Bản ghi A sẽ trỏ đến (các) máy chủ thư. Khi một máy chủ thư khác muốn liên lạc với máy chủ thư của bạn, nó sẽ tìm bản ghi MX. Bản ghi MX đó phải trỏ đến bản ghi A trỏ đến địa chỉ IP của máy chủ thư.

Bản ghi MX trỏ đến thư Một bản ghi

Nếu một bản ghi MX bị thiếu cho tên miền, thì thư cho tên miền thường sẽ được cố gắng gửi đến bản ghi A phù hợp. Vì vậy, đối với tên miền, tên của bạn là yourdomain.com, nếu không có bản ghi MX cho tên của bạn yourdomain.com thì thư sẽ được gửi đến bản ghi apex / root của Hồi giáo yourdomain.com.

Chuyển đổi bản ghi MX:

Bản ghi MX không hỗ trợ Chuyển đổi dự phòng DNS, tuy nhiên, chúng có một loại dịch vụ chuyển đổi dự phòng được tích hợp. Bạn sẽ nhận thấy rằng khi bạn tạo bản ghi MX, bạn có tùy chọn để đặt Mức MX cho bản ghi. Cấp MX xác định thứ tự (máy chủ thư) mà thư của bạn sẽ được gửi đi. Máy chủ thư có mức MX thấp nhất trước tiên sẽ được thử gửi email.

Vì vậy, nếu bạn có ba bản ghi MX với các mức 10, 20, 30, điều sau đây sẽ xảy ra:

- Thư sẽ luôn được thử trước tiên để được gửi đến bản ghi MX với Cấp độ MX là 10.
- Nếu máy chủ thư đó bị hỏng thì thư sẽ cố gắng được gửi đến máy chủ thư lúc 20.
- Nếu máy chủ thư ở cấp 20 không hoạt động thì thư sẽ được gửi đến máy chủ thư ở cấp 30.
- Nếu các máy chủ thư ở cấp 20 và 30 là máy chủ thư dự phòng thì thư sẽ được gửi đến máy chủ thư ở cấp 10 khi nó trở lại trực tuyến.
- Nếu bạn có nhiều bản ghi MX có cùng cấp MX thì nó sẽ thiết lập cấu hình robin tròn cho email của bạn.
- Máy chủ gửi email sẽ không gửi email đến cả hai máy chủ email.

## Bản ghi PTR

PTR Record (Point Record , tạm dịch: bản ghi ngược, hay còn được gọi làReverse DNS ) là một bản ghi thực hiện việc chuyển một địa chỉ IP đến tên miền.

Hiểu đơn giản, PTR record giống như một phiên bản ngược của A record: nếu A record trỏ tên miền vào một địa chỉ IP thì PTR Record trỏ một địa chỉ vào một hostname. Tuy nhien cả 2 bản ghi này làm việc hoàn toàn độc lập với nhau.

Ví dụ: nếu A record của Tinohost trỏ tới 12.12.128.xx, trong khi 13.13.128.xx trỏ tới một hostname hoàn toàn khác.

Tên miền ngược có gì khác với tên miền thông thường?

Không gian tên miền ngược cũng được xây dựng theo cơ chế phân cấp giống như tên miền thuận.

Cấu trúc: `ddd.ccc.bbb.aaa.in-addr. arpa.`

Trong đó: 

- aaa, bbb, ccc, ddd là các số viết trong hệ thập phân biểu diễn giá trị của 4 byte cấu thành địa chỉ IP.

- .arpa là mức cao nhất trong mọi không gian tên miền ngược (áp dụng với cả IPv4 và IPv6).
in-addr.arpa là mức cao nhất trong không gian tên miền ngược áp dụng với thế hệ địa chỉ IPv4.
- ip6.arpa là mức cao nhất trong không gian tên miền ngược áp dụng với thế hệ địa chỉ IPv6.

Ví dụ: Một máy tính trên mạng có địa chỉ IP là 141.021.46.28 thì tên miền ngược ứng với nó sẽ là 28.46.021.141.in-addr. arpa.

Vì sao cần có PTR Record?

- Tăng độ tin cậy của server: (hỗ trợ cho outgoing mail server)

- PTR Record sẽ cho phép điểm nhận cuối cùng đối chiếu IP của của hostname gửi tới. Đây là một cách hữu hiệu để chống lại hầu hết các hacker sử dụng tên miền giả để spam mail.

Đáp ứng yêu cầu reverse DNS lookup trước khi nhận email:

Hệ thống tên miền thông thường cho phép chuyển đổi từ tên miền sang địa chỉ IP. Trong thực tế, một số dịch vụ Internet đòi hỏi hệ thống máy chủ DNS phải có chức năng chuyển đổi từ địa chỉ IP sang tên miền. Nhiều nhà cung cấp mail lớn như https://vn.yahoo.com ,https://mail.google.com/mail luôn làm reverse DNS lookup trước nhận emails. PTR Record (tên miền ngược) ra đời nhằm phục vụ mục đích này.

Lý do bạn cần cập nhật PTR Record?

- Cần chứng thực domain đó khi phân giải ngược đúng IP server.
- Tên miền phân giải ngược đó là duy nhất cho server của bạn như mail server.

## Bản ghi DKIM

DKIM là viết tắt của DomainKeys Identified Mail. Nó hoạt động bằng cách xác minh tên miền của một email đến và chứng minh email này là thật. Giúp người dùng kiểm tra một email có xuất xứ từ một miền cụ thể được ủy quyền bởi chủ sở hữu của miền đó, chặn các địa chỉ người gửi giả.

Xét về mặt kỹ thuật, DKIM sẽ kết hợp tên miền đã đăng ký với một email bằng cách gán cho nó một chữ ký số. Công việc xác minh được thực hiện bằng cách dùng khóa công khai của người đăng ký trong DNS dưới dạng bảng ghi TXT(TXT record). Trong đó, chữ ký hợp lệ phải đảm bảo được số phần của email chưa được sửa đổi từ khi gán chữ ký vào. Chữ ký DKIM thường chỉ được cơ sở hạ tầng gắn kết hoặc xác nhận chứ không phải tác giả hay người nhận thư.

Nguyên lý:

Người gửi tạo một đoạn hash MD5 của một vài thành phần của email (ví dụ email header). Người gửi dùng một private key (chỉ có người này biết) để mã hóa đoạn MD5 hash đó. Chuỗi mã hóa được chèn vào mail, được biết đến là chữ ký DKIM. Người gửi lưu lại public key trong bản ghi DNS.

Người nhận tìm thấy public key từ DNS của tên miền đó. Người nhận sau đó dùng public key để giải mã chữ ký DKIM từ email về lại đoạn MD5 hash. Người nhận tạo ra một đoạn MD5 hash mới từ các thành phần của email được ký bởi DKIM, và so sánh nó với đoạn MD5 hash gốc. Nếu chúng khớp nhau thì người nhận sẽ biết được:
- Email được gửi từ chủ của tên miền. (Gần như không thể để giả mạo chữ ký DKIM đã giải mã về đoạn MD5 hash gốc sử dụng public key)
- Các phần tử của email được DKIM ký không bị thay đổi khi chuyển tiếp (nếu không thì đoạn MD5 hash ban đầu và đoạn MD5 hash do người nhận tạo sẽ không khớp).

Điểm thiếu sót của DKIM là nó chỉ có thể bảo vệ thư đã được ký, nhưng nó không cung cấp cơ chế để chứng minh rằng một thư chưa ký lẽ ra đã được ký.

## Bản ghi SPF

SPF (Sender Policy Framework) hoạt động với nguyên tắc xác thực một email server có được gửi email dưới tên một domain nào đó. Trong trường hợp nhận diện được email mới đến từ một địa chỉ IP không phù hợp, email sẽ được chuyển đến hộp thư Spam.

Về nguyên lý hoạt động SPF sẽ yêu cầu lập hệ thống tên miền, khai báo các máy chủ có thể gửi thư từ một miền cụ thể. Khi nhận mail, người nhận sẽ thông qua truy vấn DNS để xác thực lại địa chỉ người gửi và địa chỉ IP có phù hợp hay không, để đưa ra kết luận địa chỉ thật hay giả và có nên nhận mail hay không.

Nguyên lý:

SPF cho phép miền người gửi công bố công khai máy chủ MTA (IP) nào có thể gửi email thay mặt miền đó.

Server người nhận kiểm tra xem SPF có tồn tại trên DNS cho tên miền trong địa chỉ đến của mail (FROM). Nếu SPF có tồn tại, người nhận kiểm tra IP của server gửi có trùng với danh sách IP trong SPF không.

Điểm thiếu sót của SPF là nó xác thực máy chủ gốc chỉ xem xét tên miền trong địa chỉ MAIL FROM, không phải header thư địa chỉ from. Địa chỉ MAIL FROM là địa chỉ email mà máy chủ nhận sử dụng để thông báo cho máy chủ gửi về các vấn đề gửi. Vấn đề với hạn chế này là địa chỉ From là những gì người nhận nhìn thấy trong ứng dụng email của họ.

## Bản ghi DRMARC

DMARC là một sự nâng cấp vượt trội khi kết hợp hai chính sách bảo mật DKIM và SPF với nhau. Điều này cho phép người dùng quyền thiết lập một chính sách( policy) để loại bỏ (reject) hoặc cách ly (quarantine – cho mail vào Spam)

một email từ nguồn không có độ tin cậy dựa trên hai phương pháp DKIM và SPF.

Đối với DMARC, người dùng có thể cấu hình trên Mail Server của bên nhận cách thức xử lý khi SPF và DKIM failed. Sơ đồ mô tả cách SPF và DKIM làm việc cùng với DMARC.

    _dmarc.domain.com TXT v=DMARC1; p=reject; pct=100; rua=mailto:dmarc-reports@domain.com;

DMARC policy được cấu hình trong DNS. Thường có với giá trị reject(p=reject) 100%(pct=100) những email failed SPF và DKIM. Đồng thời, cho biết lý do từ chối đến email (rua=mailto:dmarc-reports@domain.com) để quản trị viên của miền domain.com biết.

![Imgur](https://i.imgur.com/C0zv7q6.png)

Nguyên lý: 

DMARC được xây dựng dựa trên SPF và DKIM để giải quyết những thiếu sót của hai tiêu chuẩn xác thực này. DMARC cho phép thực thi xác thực DKIM hoặc SPF và xác nhận rằng địa chỉ hiển thị From là xác thực.

Người gửi thêm chính sách DMARC trong DNS.

Người nhận
- tra cứu chính sách DMARC trong DNS;
- thực hiện xác thực chữ ký DKIM và / hoặc xác thực SPF;
- thực hiện kiểm tra liên kết miền;
- áp dụng chính sách DMARC.

Việc liên kết miền bao gồm việc xác minh rằng địa chỉ From (địa chỉ được hiển thị cho người nhận cuối cùng) khớp với:
- Với SPF, tên miền MAIL FROM
- Với DKIM, DKIM d= domain (miền ký tên được bao gồm trong header chữ ký DKIM cùng với mã hash được mã hóa)
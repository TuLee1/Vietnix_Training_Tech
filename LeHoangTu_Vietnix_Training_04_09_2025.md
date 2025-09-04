# Nội dung tìm hiểu 04/09/2025 
## SSL
### SSL là gì ?
__SSL (Secure Socket Layer)__ là một chứng chỉ bảo mật - nhưng đồng thời cũng là định nghĩa về một giao thức cho phép truyền đạt thông tin một cách bảo mật và an toàn qua mạng


### Có bao nhiêu cách xác thực SSL ?
Xác thực __SSL__ - hay còn gọi là xác thực chứng chỉ số có thể phân loại theo các mức độ xác thực. Có 3 cách xác thực SSL chính, tương ứng 3 loại chứng chỉ SSL :
1. __Domain Validation (DV)__ - Xác thực tên miền
* __Mức độ xác thực:__ thấp
* __Thời gian xác thực:__ rất nhanh (vài phút đến vài giờ)
* __Yêu cầu:__ chỉ cần chứng minh quyền sỡ hữu tên miền (qua email, DNS record hoặc file trên web server)
* __Đối tượng:__ Cá nhân, website nhỏ không yêu cầu cao về tính bảo mật
<br>

2. __Organization Validation (OV)__ - Xác thực tổ chức
* __Mức độ xác thực:__ Trung bình
* __Thời gian xác thực:__ Vài ngày
* __Yêu cầu:__
    * Đã xác thực tên miền trước đó
    * Xác minh tổ chức qua giấy phép kinh doanh, số điện thoại
* __Đối tượng:__ Doanh nghiệp, Website thương mại điện tử vừa và nhỏ, ...
<br>

3. __Extended validation (EV)__ - Xác thực mở rộng (nâng cao)
* __Mức độ xác thực:__ Cao
* __Thời gian xác thực:__ Vài ngày
* __Yêu cầu:__ 
    * Xác thực tổ chức, địa chỉ, pháp lý, ...
    * Ngoài ra tổ chức có thể được yêu cầu cung cấp thêm một số tài liệu liên quan phục vụ cho việc xác thực
*__Đối tượng:__ Website tài chính, ngân hàng, các trang thương mại điện tử lớn
<br>

### CSR File dùng để làm gì ?
__CSR File (Certificate Signing Request)__ là tệp yêu cầu ký chứng chỉ. Đây là một tệp văn bản được tạo bởi người dùng để đăng ký chứng chỉ số SSL/TLS và gửi đến cho các __CA__ (__Certificate Authority__ - Các công ty, tổ chức phát hành chứng chỉ SSL).
Một số thông tin cần thiết trong file CSR cần được cung cấp trước khi gửi yêu cầu cho CA:
* Mã quốc gia
* Thành phố
* Tên công ty
* Tên miền cần mã hóa SSL
* Email quản lý
* Loại chứng chỉ SSL  
Sau khi CSR được tạo, người dùng có thể gửi đến các bên cung cấp SSL. Nhà cung cấp sẽ sử dụng các thông tin trong file CSR để tiến hành tạo chứng chỉ và gửi lại cho người dùng
<br>

### Gen File CSR và request SSL cho domain 'tech.training.vietnix.tech' bằng OpenSSL.
__1. Tạo file key cho domain__  
```openssl genrsa -out tech.training.vietnix.tech.key 2048```  
* _genrsa_: thuật toán để tạo khóa (ở đây là RSA).
* _tech.training.vietnix.tech.key_: tên file và định dạng đầu ra *( Việc sử dụng tên domain đặt tên cho key chỉ với mục đích dễ dàng phân biệt và nhận dạng, người dùng có thể đặt tên khác )* .
* _2048:_ Độ dài của khóa. RSA 2048-bit là tiêu chuẩn tối thiểu hiện tại cho SSL/TLS để đảm bảo tính bảo mật và đang dần chuyển sang 4096 bit.  

__2. Tạo file CSR bằng OpenSSL dựa trên file key vừa được tạo__  
```openssl req -new -key tech.training.vietnix.tech.key -out tech.training.vietnix.tech.csr```
* _-new_: cờ này thể hiện là người dùng đang muốn tạo yêu cầu mới. Nếu không có cờ '-new' thì lệnh này sẽ dùng để xem hoặc phân tích file CSR/CRT đã có.
* _-key tech.training.vietnix.tech.key_: chỉ định private key cần dùng để ký CSR. Private key này được tạo ra từ trước bằng lệnh _**openssl genrsa**_. Sau khi tạo file CSR sẽ chứa Public Key được sinh ra từ Private Key này.  
* _-out - tech. training.vietnix.tech.csr_: đường dẫn và tên file CSR đầu ra. Đây là file người dùng sẽ gửi cho CA để xin cấp SSL.  

__3. Sau khi chạy câu lệnh trên người dùng sẽ tiến hành điền một số thông tin để khởi tạo file CSR__
* Country Name (2 letter code) [XX]: Country Name (Only 2 characters) 
* State or Province Name (full name) []: province/city
* Locality Name (eg, city) [Default City]: city name
* Organization Name (eg, company) [Default Company Ltd]: company name
* Organizational Unit Name (eg, section) []:
* Common Name (eg, your name or your server's hostname) []:servername or hostname
* Email Address []: email

### Pem File là gì ?
PEM (Privacy Enhanced Mail) là một định dạng tệp văn bản chứa chứng chỉ và các khóa mật mã đã được mã hóa theo định dạng Base64 ASCII  

### Private Key SSL là gì ?
Private Key SSL là file mã hóa đã được tạo ra trên thiết bị của người dùng (Ví dụ file tech.training.vietnix.tech.key mà người dùng đã tạo ở trên)  
Public Key trong File CSR được tạo ra từ Private Key này, về sau Private key sẽ là công cụ để người dùng kích hoạt SSL sau khi nhà cung cấp gửi về lại cho người dùng 

### PFX File là gì ? Cách chuyển từ CRT sang PFX
Tệp __PFX__ hay còn gọi là tệp __PKCS#12__ là tệp định dạng nhị phân có thể lưu trữ chứng chỉ, chứng chỉ trung gian và khóa riêng (__Private Key__) trong 1 file duy nhất. Nói cách khác, tệp PFX sẽ gom tất các các file .key, .crt hay .pem vào một file duy nhất giúp thuận tiện trong việc cài đặt, sử dụng, back up dữ liệu trên nhiều thiết bị khác nhau mà vẫn đảm bảo tính bảo mật của private key ở một mức độ tương đối; đồng thời việc sử dụng file PFX giúp người dùng có thể sử dụng trên hệ điều hành Window.
* Cách chuyển từ file __CRT__ sang __PFX__:  
Yêu cầu: ngoài file crt, người dùng cần thêm Private Key và chứng chỉ trung gian - CA bundle (nếu có). Người dùng có thể dùng các công cụ của các bên để chuyển đổi ra file pfx hoặc sử dụng __OpenSSL__ như dưới đây để chuyển đổi thủ công.
<pre> bash openssl pkcs12 -export -out your_domain.pfx \
 -inkey your_domain.key \ 
 -in your_domain.crt \ 
 -certfile ca_bundle.crt ``` </pre>
* _your_domain.key_: Private Key được tạo khi generate CSR (File được tạo bằng lệnh _**openssl genrsa**_).
* _your_domain.crt_: File chứng chỉ được CA cấp.
* _ca_bundle.crt_: File CA trung gian (nếu có, không thì bỏ qua trường này).
* _your_domain.pfx_: File xuất ra.

Khi chạy lệnh người dùng sẽ được yêu cầu nhập _**password**_  cho file .pfx. _**Password**_ này sẽ được yêu cầu khi file này import vào các hệ thống khác (IIS, Windows Certificate Store, ...).

## Domain

### Domain là gì?
__Domain - Tên miền__: là địa chỉ website mà người dùng nhập vào để truy cập được trang web cần tìm. Đây sẽ là một chuỗi kí tự dễ nhớ (như tên của doanh nghiệp: __vietnix.vn__, __facebook.com__, ...) với mục đích giúp người dùng dễ dàng truy cập hơn thay vì phải nhớ các dãy số (địa chỉ IP của trang web: như _172.15.23.45_) gây khó khăn cho người dùng vì với mỗi trang web thì các con số trên lại khác nhau 
### Các trạng thái của domain.
Các trạng thái của domain có thể chia thành 2 phần chính:
* Trạng thái tên miền ở __Registry__: __Registry__ là đơn vị cấp phát cho một  __TLD (Top-level Domain)__ cụ thể như .com, .net, .org, ... <br>

| Trạng thái           | Ý nghĩa |
| :-------------       |:--------|
|OK / active           | Tên miền hoạt động bình thường sau khi đăng ký|
| addPeriod            | Trạng thái sau khi tên miền mới vừa được đăng ký|
| autoRenewPeriod      | Thời gian đăng ký tự động gia hạn tên miền. Trạng thái này cho phép đơn vị đăng ký có thể hủy việc gia hạn tên miền nhưng sẽ mất một khoản phí cho nhà cung cấp|
|inactive              | Tên miền đã được đăng ký nhưng NameServer chưa kết nối được với tên miền của người dùng|
|pendingCreate         | Đang chờ Đăng ký|
|pendingDelete         | Tên miền hết hạn đăng ký. Chuẩn bị xóa
|pendingRenew          | Đang chờ gia hạn
|pendingRestore        | Tên miền đã hết hạn và đang chờ được khôi phục (về trạng thái Active). Nếu trong thời gian này đơn vị đăng ký không thực hiện yêu cầu khôi phục nào, trạng thái của tên miền sẽ chuyển sang trạng thái **redemptionPeriod**
|pendingTransfer       |Đang chờ chuyển đổi đơn vị cung cấp (Registar)|
|pendingUpdate         |Đang chờ cập nhật (các thông tin liên quan đến tên miền)|
|redemptionPeriod      |Tên miền đã hết hạn và rơi vào trạng thái cần đóng phí bổ sung để khôi phục. Thời gian để có thể khôi phục tên miền là trong khoảng __30 ngày__|
|renewPeriod           |Trạng thái này được đặt trong khoảng thời gian giới hạn để xác nhận việc gia hạn tên miền của người dùng với Registry|
|serverDeleteProhibited|Trạng thái ngăn tên miền bị xóa. <br> Trạng thái này được sử dụng khi tên miền đang trong quá trình tranh chấp pháp lý, hoặc khi đang trong trạng thái redemptionPeriod. |
|serverHold            |Tên miền không được kích hoạt trong DNS|
|serverRenewProhibited |Trạng thái không thể gia hạn tên miền. <br> Tương tự với __serverDeleteProhibited__ trạng thái này chỉ xuất hiện khi tên miền đang bị tranh chấp pháp lý giữa các bên|
|serverUpdateProhibited|Trạng thái không cho phép cập nhật tên miền <br> Tương tự với 2 trạng thái __Prohibited__ chỉ xuất hiện khi đang có tranh chấp về tên miền|
|TransferPeriod        |Trạng thái cho phép sau khi transfer tên miền thành công thì nhà đăng ký mới có thể yêu cầu nhà cung cấp xóa tên miền|

<br>
<br>
<br>

* Trạng thái tên miền ở __Registar__ - đơn vị quản lý tên miền: đây là đơn vị được ủy quyền để cung cấp dịch vụ tên miền cho người dùng (ví dụ như __Vietnix__)<br> 

| Trạng thái| Ý nghĩa|
|:----------|:-------|
|clientDeleteProhibited| Trạng thái không cho phép xóa tên miền (Cấm hủy domain). Trạng thái này giúp người dùng tránh xóa nhầm hoặc bị chiếm quyền và xóa tên miền|
|clientHold| Trạng thái này cho biết tên miền của người dùng không hoạt động. Vấn đề này có thể phát sinh trong quá trình hoàn thành hồ sơ đăng ký tên miền với đơn vị quản lý.
|clientRenewProhibited| Trạng thái không cho phép gia hạn tên miền. Tương tự như các trạng thái __Prohibited__ thì trạng thái này do tên miền đang trong quá trình tranh chấp pháp lý. Người dùng có thể liên hệ với đơn vị quản lý để giải quyết vấn đề và có thể xóa trạng thái này|
|clientTransferProhibited| Trạng thái không cho phép transfer (chuyển đổi đơn vị quản lý) tên miền. Trạng thái này giúp tên miền của người dùng không bị chuyển đổi trái phép do mất quyền điều khiển hoặc bị tấn công. Người dùng có thể liên hệ với đơn vị quản lý để xóa trạng thái này|
|clientUpdateProhibited| Trạng thái không cho phép cập nhật thông tin tên miền. Trạng thái giúp ngăn chặn các thay đổi trái phép do mất quyền điều khiển hoặc bị tấn công. Người dùng có thể liên hệ với đơn vị quản lý để xóa trạng thái này|
### Subdomain là gì?
__Subdomain__ là phần mở rộng phía trước của tên miền chính và được ngăn cách bới dấu '.' cho phép người dùng tạo ra các phân mục hoặc các website riêng nhưng vẫn nằm trong sự quản lý của tên miền chính
Ví dụ:
* Domain: __vietnix.vn__ --> Subdomain: __blog.vietnix.vn__  

Vai trò:
* Sử dụng __Subdomain__ giúp tạo ra các website hoặc các khu vực con với nội dung, thiết kế và nền tảng công nghệ độc lập, không bị phụ thuộc vào website chính
* Mỗi __Subdomain__ trỏ về một thư mục gốc (Root Document) riêng trên máy chủ giúp việc quản lý hay cập nhật thay đổi cho __Subdomain__ không ảnh hưởng đến các __Subdomain__ khác hay __Domain__ chính
* Dù độc lập về nội dung hay thiết kế nhưng các __Subdomain__ vẫn là một phần của tên miền chính về mặt sở hữu và quản lý __DNS__. Tuy nhiên việc tạo __Subdomain__ thường sẽ miễn phí và không giới hạn số lượng (tùy thuộc vào nhà cung cấp tên miền) giúp giảm chi phí so với việc đăng ký một tên miền mới.
### Virtual Hosts là gì?
Là tính năng trong webserver và cũng là phương pháp lưu trữ cho phép nhiều trang web và tên miền hoạt động trên cùng một máy chủ vật lý hoặc 1 địa chỉ IP duy nhất  

* __Ưu điểm:__  
    * Tiết kiệm chi phí: so với việc sử dụng máy chủ riêng (dedicated server), việc sử dụng Virtual Hosts giúp tối ưu chi phí về việc vận hành cũng như sử dụng được hết tài nguyên của server, từ đấy trở thành giải pháp phù hợp cho nhiều đối tượng người dùng hay doanh nghiệp khác nhau
    * Dễ quản lý: Việc cho phép lưu trữ và vận hành nhiều website khác nhau trên cùng một server giúp việc quản lý dễ dàng hơn khi người dùng chỉ cần cài đặt các công cụ quản lý (như cPanel, DirectAdmin, ...) lên một server để thiết lập  và quản lý cho nhiều website cùng một 
    * Quản lý độc lập: Tuy sử dụng chung tài nguyên từ một máy chủ nhưng các Virtual Hosts đều có file cấu hình riêng từ đó cho phép người dùng linh hoạt trong việc thay đổi hay cấu hình cho trang web mà không ảnh hưởng đến các website khác đang hoạt động.
* __Nhược điểm:__
    * Tài nguyên và hiệu năng: Do cùng sử dụng một máy chủ và tài nguyên được chia sẻ, nên hiệu năng sẽ có giới hạn và không phù hợp với các website yêu cầu cao về mặt tài nguyên.
    * Bảo mật: Do cùng dùng chung một máy chủ nên khi một website bị tấn công - máy chủ bị tấn công thì các website dùng chung cũng có thể bị ảnh hưởng.
    * Giới hạn về quyền: Người dùng sẽ không có quyền truy cập cao nhất vào máy chủ (root), từ đó ảnh hưởng đến việc tùy chỉnh hoặc can thiệp sâu vào hệ thống website mà cần phải liên hệ với đơn vị cung cấp

## Mail Server
__Mail Server__ là hệ thống máy chủ được cấu hình cho các doanh nghiệp hoặc tổ chức để nhận và gửi thư điện tử trong nội bộ và bên ngoài một cách chuyên nghiệp  
__Cách hoạt động của Mail Server:__
* __Outgoing Mail Server__ - Mail Server gửi đi: sử dụng phương thức SMTP (Simple Mail Transfer Protocol) để giao tiếp với các Server từ xa và từ đó giúp người dùng gửi nhiều mail một lúc đến các server khác nhau.
* __Incoming Mail Server__: hoạt động thông qua 2 hình thức
    * __POP3 (Post Office Protocol)__: Lưu email đến tại thiết bị của người dùng bằng những ứng dụng phổ biến như Outlook, Mac Mail hoặc Windows Mail, ...
    * __IMAP (Internet Message Access Protocol)__: email đến sẽ được lưu tại một Mailbox - cho phép nhiều người dùng cùng truy cập đến để sao chép về client.
### Tìm hiểu MX Record.
__MX Record__ là một bản ghi DNS gắn liền với tên miền giúp định tuyến cho các email được gửi đến tên miền sẽ được gửi về __Mail Server__ nào  
__Cấu trúc của một MX Record:__
* Priority (độ ưu tiên): giá trị càng thấp, mức độ ưu tiên càng cao (số nguyên)
* Mail Server (hostname): tên máy chủ email(phải có bản ghi A hoặc CName trỏ IP).  
__Ví dụ:__  
> Vietnix.com.     IN MX 10 mail1.vietnix.com.  
  Vietnix.com.     IN MX 20 mail2.vietnix.com.  

Email gửi đến tên miền __Vietnix.com__ sẽ được gửi tới __mail1.vietnix.com__ trước (Vì __Priority = 100__)  
Nếu __mail1__ không hoạt động thì email sẽ được gửi đến __mail2__.
### Tìm hiểu DKIM, SPF, PTR.
Trước khi tìm hiểu về DKIM, SPF và PTR , ta cần hiểu về một khái niệm khác là TXT Record.  
TXT Record là một loại bản ghi DNS cho phép chủ sở hữu tên miền thêm chuỗi văn bản (text) vào DNS của domain.

* __DKIM:__  
__DKIM (DomainKeys Identified Mail)__ là phương thức giúp xác nhận Email thông qua chữ ký số để chống email giả dạng, lừa đảo hay spam.  
__Cách hoạt động:__
    * __Ở bên gửi__:
        * Tạo cặp khóa public / private key (có thể sử dụng phần mềm Openssl để tạo)
        * Chuyển khóa public lên khai báo bản ghi TXT trên DNS, với tương ứng domain được dùng để gửi mail
        * Cấu hình Mail Server sử dụng private key để kí vào email trước khi gửi đi (private key chỉ lưu trên Mail Server nên không thể làm giả)
    * __Ở bên nhận__:
        * Nhận email từ bên gửi và kiểm tra email có thông điệp được mã hóa do cấu hình DKIM
        * Query DNS để lấy khóa public và giải mã email. Nếu giải mã không thành công thì dựa vào chính sách bên nhận để từ chỗi hoặc vẫn nhận mail.
* __SPF:__  
__SPF - Sender Policy Framework__ là công nghệ xác thực mail bằng cách giúp chủ tên miền chỉ định xem hostname, máy chủ nào được phép gửi email thay cho tên miền của họ  
TXT SPF record là bản ghi DNS liệt kê tất cả hostname/địa chỉ IP, máy chủ được ủy quyền và thay cho domain để gửi email

* __PTR:__  
PTR là một loại bản ghi DNS được sử dụng cho việc tra ngược DNS (__Reverse DNS Lookup__) giúp ánh xạ ngược lại từ địa chỉ IP sang tên miền (ngược lại với a record)  
__Công dụng:__
    * Đáp ứng yêu cầu reverse DNS lookup khi nhận email của một số dịch vụ trên Internet
    * Tăng độ tin cậy cho server: ở bên nhận email có thể dựa vào PTR để đối chiếu địa chỉ IP với tên nhận của máy chủ gửi mail từ đó giúp ngăn chặn thư rác.





## DNS
### DNS là gì ?
__DNS - Domain Name System:__ là hệ thống phân giải tên miền - cho phép thiết lập tương ứng giữa địa chỉ IP và tên miền trên Internet

### Các loại record DNS.
Một số DNS Record phổ biến:
* A: ánh xạ tên miền đến địa chỉ IP (IPv4)
* AAAA: tương tự như A nhưng được dùng cho IPv6
* MX: chỉ định máy chủ Mail Server chịu trách nhiệm nhận mail gửi về tên miền
* CName (Canonical Name): dùng để trỏ một tên miền sang một tên miền khác thay vì trỏ trực tiếp đến địa chỉ IP.
* TXT: chứa các thông tin văn bản bổ sung, thường dùng để xác thực danh tính tên miền hoặc dùng để lưu các bản ghi như SPF 
### Nguyên tắc làm việc và cách phân giải địa chỉ của DNS
Khi người dùng tiến hành gõ một địa chỉ vào thanh tìm kiếm ví dụ __vietnix.vn__ thì quá trình phân giải tên miền của DNS sẽ diễn ra theo các bước:
* Trình duyệt và hệ điều hành: Trình duyệt sẽ kiểm tra xem đã có bản ghi DNS cho tên miền chưa nếu có và còn TTL thì sẽ sử dụng. Nếu trình duyệt không có thì, hệ điều hành sẽ bắt đầu tìm kiếm và thực hiện tương tự
* Nếu trên thiết bị (DNS cục bộ) không có thông tin thì yêu cầu truy vấn sẽ được gửi đến DNS Resolver (như Google hay Cloudfare)
* Nếu trong cached của Resolver chưa có thông tin thì yêu cầu truy vấn sẽ được gửi đến __Root Server__. 
* __Root Server__ sẽ trả về thông tin của __TLD Server__ (Top-level Domain).
* __TLD Server__ sẽ trả về thông tin của __Authoriative Name Server__
* __Authoriative Name Server__ sẽ trả về __A record__ có chứa địa chỉ IP tương ứng với tên miền được yêu cầu__
* Sau khi đã nhận được máy tính sẽ lưu và cache (theo TTL để sử dụng cho những lần sau) và bắt đầu kết nối đến IP  

Nguyên tắc hoạt động DNS:
* Cấu trúc phân cấp: Root -> TLD -> Authoriative 
* Mỗi bước đều sẽ lưu vào cache để có thể tái sử dụng và tiết kiệm thời gian truy vấn.

<br>


## Linux Command Line
### Ping vietnix.vn và giải thích kết quả lệnh 'ping' và 'hping3'.
#### __Ping:__
```
ping vietnix.vn
PING vietnix.vn (103.90.224.90) 56(84) bytes of data.
64 bytes from 103.90.224.90: icmp_seq=1 ttl=53 time=2.86 ms
64 bytes from 103.90.224.90: icmp_seq=2 ttl=53 time=11.7 ms
64 bytes from 103.90.224.90: icmp_seq=3 ttl=53 time=18.8 ms
64 bytes from 103.90.224.90: icmp_seq=4 ttl=53 time=18.7 ms
64 bytes from 103.90.224.90: icmp_seq=5 ttl=53 time=3.30 ms
64 bytes from 103.90.224.90: icmp_seq=6 ttl=53 time=6.48 ms
64 bytes from 103.90.224.90: icmp_seq=7 ttl=53 time=3.31 ms
64 bytes from 103.90.224.90: icmp_seq=8 ttl=53 time=6.95 ms
64 bytes from 103.90.224.90: icmp_seq=9 ttl=53 time=6.08 ms
64 bytes from 103.90.224.90: icmp_seq=10 ttl=53 time=6.02 ms
64 bytes from 103.90.224.90: icmp_seq=11 ttl=53 time=8.12 ms
64 bytes from 103.90.224.90: icmp_seq=12 ttl=53 time=6.03 ms
64 bytes from 103.90.224.90: icmp_seq=13 ttl=53 time=2.94 ms
64 bytes from 103.90.224.90: icmp_seq=14 ttl=53 time=20.1 ms
64 bytes from 103.90.224.90: icmp_seq=15 ttl=53 time=3.11 ms
^C
--- vietnix.vn ping statistics ---
15 packets transmitted, 15 received, 0% packet loss, time 14018ms
rtt min/avg/max/mdev = 2.856/8.296/20.080/5.912 ms
```
Khi chạy lệnh Ping thì các gói ICMP sẽ được gửi đi để kiểm tra kết nối giữa host và client.  
Các thông tin mà người dùng quan tâm khi kết quả trả về như:
* Địa chỉ IP của tên miền: 103.90.224.90
* icmp_seq: các gói icmp đã được gửi đi lần lượt đánh số từ 1 và tăng dần
* ttl: là thời gian tồn tại của gói tin trên môi trường mạng để tránh tình trạng gói tin di chuyển vô thời hạn giữa các router trong mạng
* time: thời gian nhận được phản hồi
#### Hping3:
```
sudo hping3 -S -p 80 vietnix.vn
HPING vietnix.vn (wlp1s0 14.225.253.240): S set, 40 headers + 0 data bytes
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=0 win=65535 rtt=3.8 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=1 win=65535 rtt=7.7 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=2 win=65535 rtt=8.6 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=3 win=65535 rtt=6.4 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=4 win=65535 rtt=9.1 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=5 win=65535 rtt=7.0 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=6 win=65535 rtt=5.9 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=7 win=65535 rtt=6.8 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=8 win=65535 rtt=6.8 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=9 win=65535 rtt=7.6 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=10 win=65535 rtt=11.5 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=11 win=65535 rtt=3.4 ms
len=46 ip=14.225.253.240 ttl=53 DF id=0 sport=80 flags=SA seq=12 win=65535 rtt=8.3 ms
^C
--- vietnix.vn hping statistic ---
13 packets transmitted, 13 packets received, 0% packet loss
round-trip min/avg/max = 3.4/7.1/11.5 ms
```
Khác với __ping__, __hping3__ có nhiều tùy chọn với các flag khác nhau cho phép người dùng có thể lựa chọn gửi gói tin tcp, udp hay tracerout.  
Câu lệnh ở trên khi được thực thi sẽ tiến hành gửi các gói TCP với cờ SYN được bật đến port 80.  
Trong đó:
* len: kích thước gói tin
* ip: địa chỉ ip phản hồi
* ttl: Time-to-live
* DF: "Don't Fragment" - cờ trong IP Header, báo gói tin không được phân mảnh
* id
* sport: server port - cổng nguồn của server
* flags: S = SYN, A = ACK -> đây là gói SYN ACK thông báo rằng port 80 đang mở và sẵn sàng nhận kết nối
* seq: số thứ tự gói tin
* win: kích thước cửa sổ gói TCP
* rtt: Round Trip Time - thời gian khứ hồi 
---

### SSH Command:
#### Kết nối bằng password: 
```
ssh user@host
```
Sau khi kết nối vào hệ thống sẽ yêu cầu nhập password của user
#### Kết nối bằng key:
```
ssh -i /path/to/private_key user@host
```
#### Kết nối bằng port custom
```
ssh -p port_number user@host
```
---

### SCP Command:
#### Copy một file:
```
scp file.txt user@host:/remote/path/
```
#### Copy một folder:
```
scp -r folder/ user@host:/remote/path/
```
---

### Rsync Command:
#### Copy file:
```
rsync file.txt user@host:/remote/path/
```

#### Copy folder:
```
rsync -av folder/ user@host:/remote/path/
```

#### Rsync incremental:
```
rsync -av --progress source/ destination/
```
(Tự động chỉ copy các file thay đổi, tiết kiệm băng thông)

---

### Cat Command:
#### Xem nội dung 1 file:
```
cat file.txt
```

#### Xem dòng thứ <n> trong file:
```
sed -n '<n>p' file.txt
```

#### Ghi nhiều dòng vào 1 file bằng EOF:
```
cat <<EOF > file.txt
dong 1
dong 2
EOF
```

---

### Echo Command:
#### Chèn thêm 1 dòng vào cuối file:
```
echo "Noi dung moi" >> file.txt
```

#### Overwrite nội dung file:
```
echo "Noi dung moi" > file.txt
```

---

### Tail/Head Command:
#### Sự khác biệt giữa tail và head:
- head: Xem những dòng đầu của file.
- tail: Xem những dòng cuối của file.

#### Sự khác biệt giữa tail và tailf:
- tail -f: Liên tục theo dõi file (ví dụ log).
- tailf: Giống tail -f nhưng tối ưu cho file log (ít tốn tài nguyên).

---

### Sed Command:
#### Find and replace string trong file:
```
sed -i 's/chu_cu/chu_moi/g' file.txt
```

---

### Traceroute/Tracert Command:
#### Thực hiện:
```
traceroute vietnix.vn   # Linux/Mac
tracert vietnix.vn      # Windows
```

---

### Netstat Command:
#### Hiển thị các socket đang listen:
```
netstat -l
```

#### Không resolve hostname:
```
netstat -n
```

#### Không resolve portname:
```
netstat -n
```

#### Display process name/PID:
```
netstat -p
```

#### Chỉ hiển thị socket TCP:
```
netstat -at
```

#### Chỉ hiển thị socket UDP:
```
netstat -au
```

---

### Sort Command:
#### Theo thứ tự tăng dần:
```
sort file.txt
```

#### Theo thứ tự giảm dần:
```
sort -r file.txt
```

#### Theo column:
```
sort -k 2 file.txt
```

---

### Uniq Command:
#### Lọc các dòng lặp lại:
```
uniq file.txt
```

#### Lọc và đếm số lượng dòng lặp lại:
```
uniq -c file.txt
```

---

### Wc Command:
#### Đếm số dòng:
```
wc -l file.txt
```

#### Đếm số ký tự:
```
wc -m file.txt
```

---

### Chmod, Chown, Chattr Command:
#### Phân quyền bằng số và chữ:
```
chmod 755 file.txt
chmod u+x file.txt
```

#### Đổi owner user/group:
```
chown user:group file.txt
```

#### Set Immutable Attribute:
```
chattr +i file.txt
```

---

### Find Command:
#### Tìm file đuôi .log:
```
find /path -type f -name "*.log"
```

#### Tìm folder tên abc:
```
find /path -type d -name "abc"
```

#### Tìm file tên abc:
```
find /path -type f -name "abc"
```

#### Tìm file abc và đặt quyền read only:
```
find /path -type f -name "abc" -exec chmod 444 {} \\;
```

---

### Cp Command:
#### Copy file:
```
cp file.txt /path/to/destination/
```

#### Copy folder:
```
cp -r folder/ /path/to/destination/
```

---

### Mv Command:
#### Di chuyển/đổi tên file/folder:
```
mv file.txt /path/to/destination/
mv oldname.txt newname.txt
```

---

### Cut Command:
#### Lấy ký tự thứ <n>:
```
cut -c <n> file.txt
```

#### Lấy từ ký tự <n> trở về sau:
```
cut -c <n>- file.txt
```

#### Lấy đến ký tự thứ <n>:
```
cut -c -<n> file.txt
```

---

### Dig Command:
#### Kiểm tra record A, MX, NS:
```
dig A vietnix.vn
dig MX vietnix.vn
dig NS vietnix.vn
```

#### Kiểm tra record A, MX, NS với custom DNS:
```
dig @8.8.8.8 A vietnix.vn
dig @8.8.8.8 MX vietnix.vn
dig @8.8.8.8 NS vietnix.vn
```

---

### Tar/Zip/Unzip Command:
#### Nén/giải nén tar.gz:
```
tar -czvf archive.tar.gz folder/
tar -xzvf archive.tar.gz
```

#### Nén/giải nén .zip:
```
zip -r archive.zip folder/
unzip archive.zip
```

---

### Mount/Umount Command:
#### Thêm ổ cứng sdb ~ 5gb:
```
fdisk /dev/sdb
mkfs.ext4 /dev/sdb1
```

#### Kiểm tra số lượng ổ cứng:
```
lsblk
```

#### Mount vào /mnt/test:
```
mount /dev/sdb1 /mnt/test
```

#### Umount /mnt/test:
```
umount /mnt/test
```

---

### Symbolic Links, Hard Links Command:
#### Định nghĩa Sym Link:
Shortcut trỏ đến file thật. Nếu file gốc bị xóa, link sẽ hỏng.

#### Định nghĩa Hard Link:
Bản sao tham chiếu trực tiếp vào inode. File vẫn tồn tại dù xóa file gốc.

#### Ví dụ:
```
ln -s file.txt sym_link.txt   # symbolic link
ln file.txt hard_link.txt     # hard link
```

---

### Ls Command:
#### Liệt kê file/thư mục:
```
ls
```

#### Liệt kê file/thư mục và thuộc tính:
```
ls -l
```

#### Show file ẩn:
```
ls -a
```

---

### Ps Command:
#### Show tiến trình:
```
ps aux
```

#### Kill tiến trình:
```
kill -9 PID
```

---

### Top Command:
#### Kiểm tra tài nguyên CPU:
```
top
top - 17:02:08 up  8:32,  1 user,  load average: 0,97, 0,82, 0,79
Tasks: 374 total,   1 running, 371 sleeping,   0 stopped,   2 zombie
%Cpu(s):  0,9 us,  0,8 sy,  0,0 ni, 98,2 id,  0,0 wa,  0,0 hi,  0,1 si,  0,0 st 
MiB Mem :  15364,0 total,   3949,7 free,   6353,8 used,   5455,3 buff/cache     
MiB Swap:   2048,0 total,   2048,0 free,      0,0 used.   9010,2 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                                                                                                                                
   7027 tulehoa+  20   0 3895712   1,2g 143340 S   5,0   8,2  30:26.12 Isolated Web Co                                                                                                                                                        
    167 root     -51   0       0      0      0 S   1,7   0,0   2:11.99 irq/35-ACPI:Event                                                                                                                                                      
   1185 root      20   0 1442528 143168  84356 S   1,7   0,9   7:58.09 Xorg            
```

#### Giải thích các thông số:
- PID: ID tiến trình
- %CPU: phần trăm CPU sử dụng
- %MEM: phần trăm RAM sử dụng
- TIME+: thời gian CPU đã dùng
- COMMAND: lệnh thực thi

---

### Free Command:
#### Giải thích các thông số về RAM:
```
free -m
               total        used        free      shared  buff/cache   available
Mem:           15364        6394        3910          69        5454        8969
Swap:           2047           0        2047

```
- total: tổng dung lượng RAM
- used: đã dùng
- free: còn trống
- buff/cache: RAM cho cache
- available: RAM còn dùng được

---

### Df Command:
#### Xem dung lượng disk:
```
df -h
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           1,6G  1,9M  1,5G   1% /run
efivarfs        128K   45K   79K  37% /sys/firmware/efi/efivars
/dev/nvme0n1p7   98G   18G   75G  20% /
tmpfs           7,6G   24K  7,6G   1% /dev/shm
tmpfs           5,0M   16K  5,0M   1% /run/lock
/dev/nvme0n1p1  296M   38M  259M  13% /boot/efi
tmpfs           1,6G  3,8M  1,5G   1% /run/user/1000

```

#### Phân vùng / là gì:
```
- / là root filesystem, chứa toàn bộ hệ thống Linux.
```

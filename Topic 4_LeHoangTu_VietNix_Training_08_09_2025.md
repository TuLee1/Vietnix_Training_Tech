# NỘI DUNG TÌM HIỂU 08/09/2025 
<br>


## XÂY DỰNG MÔ HÌNH REVERSE PROXY KẾT HỢP NGINX VÀ APACHE

### 1. Chuyển đổi port của Apache:
Do cả apache và nginx đều sử dụng port 80/443 (HTTP/HTTPS) và trong hệ thống này thì nginx sẽ đứng trước nên người dùng sẽ đổi port lắng nghe của apache để tránh xung đột
```
sudo nano /etc/apache2/ports.conf
```

<img width="705" height="336" alt="Image" src="https://github.com/user-attachments/assets/34722afc-7674-44a1-9595-43388e486b1a" />

Ở trong file người dùng sẽ thay đổi port 80/443 thành 8080/8081 (hoặc bất kì port nào khác còn trống )  

### 2. Cấu hình Virtual Host cho 2 website
Tiến hành cấu hình để 2 website có thể chạy được trên apache
* __WordPress__
<img width="776" height="602" alt="Image" src="https://github.com/user-attachments/assets/e4030879-6d24-4e40-a797-a9fac87f21db" />  

<br>
<br>
  

* __Laravel__
<img width="888" height="660" alt="Image" src="https://github.com/user-attachments/assets/58727eba-1b60-4c9d-8373-f9205e214c88" />

### 3. Cấu hình Nginx
Tiến hành cấu hình để Nginx trở thành Reverse Proxy - tiếp nhận tất cả request rồi mới gửi về cho Apache.
* __WordPress__
<img width="701" height="670" alt="Image" src="https://github.com/user-attachments/assets/a70b3b08-6d5e-4739-a9e8-20617028e781" />

<br>

* __Laravel__
<img width="702" height="651" alt="Image" src="https://github.com/user-attachments/assets/5b627aa4-3d53-4ac3-bf37-18e7d52f715a" />  

<br>
<br>

  
* Ở 2 file này các `proxy_` chính là những gì người dùng cấu hình để __Nginx__ trở thành __Reverse Proxy__.
Trong các __proxy__ này thì dòng `proxy_pass`: sẽ trỏ request từ bên ngoài đến địa chỉ cần đến back end. Việc này giúp Apache chỉ cần xử lý các request ở port 443 -> port 8081  
* Ngoài ra nếu người dùng không sử dụng tùy chọn `return 301 ...` (chuyển request HTTP tự động sang HTTPS) thì có thể cấu hình tương tự của server 443 để các request từ port 80 của nginx sẽ đẩy về port 8080 tương ứng của apache.
* Tùy chọn `location ~* \.(jpg|jpeg|png|gif|ico|css|js|pdf|webp|svg|woff2?|ttf|mp4)`: được cấu hình để Nginx xử lý các file tĩnh mà không cần chuyển sang Apache giúp giảm tải cho hệ thống.

<br>

### Nginx đặt trước Apache:
* Việc đặt nginx phía trước Apache dưới dạng __reverse proxy__ giúp tận dụng được khả năng xử lý các request từ người dùng đặc biệt là các file tĩnh (html, css, png, ...). Khi gặp các nội dung động (ví dụ như các gói PHP), Nginx sẽ gửi request cho Apache xử lí và trả về kết quả hiển thị. Việc kết hợp Nginx và Apache sẽ giúp giảm tải cho hệ thống thay vì Apache sẽ phải tiếp nhận và xử lý tất cả request.
* Việc sử dụng cả 2 dịch vụ sẽ tận dụng được tối ưu điểm mạnh của cả Nginx (nhanh, gọn, tối ưu trong kết nối) và Apache (linh hoạt, phù hợp với nhiều ứng dụng PHP như Laravel, WordPress).
* Ngoài ra trong trường hợp có nhiều server backend thì Nginx có cả load balancer giúp phân bổ request đều cho các máy chủ -> Giúp hệ thống hoạt động ổn định hơn và duy trì tính liên tục của hệ thống.

### 4. Cấu hình mod_rpaf
Module này được cài để viết lại các giá trị của REMOTE_ADDR, HTTPS và HTTP_PORT dựa trên cái giá trị do reverse proxy (nginx) cung cấp. Module này giúp hệ thống không cần phải sửa mã nguồn để có thể chạy các ứng dụng PHP  

* Cài đặt module từ github (phiên bản mới nhất và được cập nhật liên tục) thay vì trong kho của ubuntu (bản cũ)
```
wget https://github.com/gnif/mod_rpaf/archive/stable.zip
```
* Sao đó người dùng tiến hành cài đặt
```
unzip stable.zip    #Giải nén file vừa tải về
cd mod_rpaf-stable
make
sudo make install   #biên dịch và cài đặt module
```
* Tạo file trong thư mục __mods-available__ để tải module rpaf:
```
sudo nano /etc/apache2/mods-available/rpaf.load
```
Sao đó thêm nội dung 
`LoadModule rpaf_module /usr/lib/apache2/modules/mod_rpaf.so`. Sau đó lưu và thoát  

* Tạo file cấu hình cho mod_rpaf:
```
sudo nano /etc/apache2/mods-available/rpaf.conf
```

Nội dung:  

<img width="392" height="217" alt="Image" src="https://github.com/user-attachments/assets/407d6905-9da1-44df-a57a-9ae439556af5" />

`RFAP_ProxyIPs`: Người dùng nhập địa chỉ IP của server Apache

* Sau đó kích hoạt module và restart lại Apache:
```
sudo a2enmod rpaf
sudo apachetl -t
sudo systemctl reload apache2
```

### 5. SSL
Người dùng sử dụng lại SSL đã được ZeroSSL cấp trước đó. Về cách cấu hình thì ở phần __Apache__ và __Nginx__, người dùng có thể thấy các cấu hình liên quan đến __ssl__ . Sau khi cài thành công thì SSL cũng đã áp dụng thành công lên 2 domain.
#### Apache:
* WordPress:  
  <img width="718" height="321" alt="Image" src="https://github.com/user-attachments/assets/3fefa821-10ec-40d9-9ba7-04033a9d8406" />

  <br>
  
* Laravel:  
  <img width="957" height="640" alt="Image" src="https://github.com/user-attachments/assets/db5a7760-d66f-4ec6-997e-a72043cd1d7c" />

#### Nginx
* WordPress:
<img width="702" height="651" alt="Image" src="https://github.com/user-attachments/assets/5b627aa4-3d53-4ac3-bf37-18e7d52f715a" />

<br>

* Laravel:
<img width="701" height="670" alt="Image" src="https://github.com/user-attachments/assets/a70b3b08-6d5e-4739-a9e8-20617028e781" />

<br>


Việc cấu hình SSL ở cả Nginx và Apache sẽ tạo ra kết nối SSL __end-to-end__ giúp tăng tính bảo mật vì khi đó dữ liệu sẽ được mã hóa trong suốt quả trình từ client đến backend để xử lí.

---

## Tạo Default VHost cho VPS
* __Đặt Vhost ở Nginx__: Vì nginx sẽ nhận tất cả request và xử lý tại đây, giúp giảm lưu lượng và giúp apache không phải xử lý các request không cần thiết.

* Tạo thư mục và file để lưu trang __default vhost__
```
sudo mkdir -p /var/www/default/public
echo "<h1>Default vhost - HTTP/HTTPS</h1>" | sudo tee /var/www/default/public/index.html #file html sẽ được hiển thị khi vhost được truy cập
```

* Cấu hình default vhost trong Nginx: `/etc/nginx/sites-available/default`  

Nội dung: 
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    server_name _;

    root /var/www/default/public;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

# Default vhost HTTPS
server {
    listen 443 ssl default_server;
    listen [::]:443 ssl default_server;

    server_name _;

    root /var/www/default/public;
    index index.html;

    ssl_certificate /etc/ssl/certs/default.crt;
    ssl_certificate_key /etc/ssl/private/default.key;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

* __Tạo certificate để cấu hình SSL cho default vhost__: Vì default vhost không cần SSL hợp lệ nên người dùng có thể tạo __self-signed-certificate__ bằng __openssl__:
```
sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /etc/ssl/private/default.key \
  -out /etc/ssl/certs/default.crt \
  -subj "/C=VN/ST=HN/L=HN/O=Default/OU=IT/CN=default.local"
```
Câu lệnh trên sẽ tạo ra các file cần thiết để cài ssl và lưu và đúng đường dẫn ssl trước đó được khai báo trong file cấu hình __default__

* Kích hoạt site
```
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
Sau đó người dùng có thể truy cập trực tiếp bằng địa chỉ IP của VPS và dù là HTTP hay HTTPS thì kết quả đều sẽ trả về default VPS như hình dưới
<img width="951" height="148" alt="Image" src="https://github.com/user-attachments/assets/c148c721-513a-4292-b3aa-e7325fd2a119" />

<img width="961" height="225" alt="Image" src="https://github.com/user-attachments/assets/e8e28151-7727-4620-bec4-c488a98b6814" />
Ở sẽ có thông báo "Không an toàn" bởi vì trong trường hợp này người dùng sử dụng chứng chỉ tự kí (tạo bằng openssl). Để xóa trạng thái này người dùng có thể sử dụng các bên thứ 3 như ZeroSSL, Let's Encrypt.

---
## DEMO
Ở trong phần demo này, website Laravel sẽ sử dụng website có cả các file tĩnh (css, png, ...) và file động (php)  

Nguồn: https://github.com/bagisto/bagisto (Trang web bán hàng)  

Người dùng tiến hành tải các tài nguyên về và cài lên domain (domain được sử dụng ở đây là laravel.tule.vietnix.tech) 

Giao diện web:  

<img width="1671" height="803" alt="Image" src="https://github.com/user-attachments/assets/6e4ed95f-f51f-4941-a6e4-0719e06ce480" />  

<br>

Để thuận tiện hơn cho việc chạy thử và kiểm tra người dùng có thể thêm header để kiểm tra xem file đó là được xử lý bới __Nginx__ hay __Apache__ : bằng 2 dòng dưới đây cho cả file config của __Apache__ và __Nginx__

<img width="740" height="691" alt="Image" src="https://github.com/user-attachments/assets/2a32ae54-f44e-4bd8-8864-7af7c9277362" />
<img width="712" height="651" alt="Image" src="https://github.com/user-attachments/assets/63db1598-8fe7-4ec0-ab22-cc84330c260b" />

Việc thêm các "header" này chỉ nhằm mục đích chạy thử và dễ dàng hơn trong việc xác định các request được xử lý bằng __Nginx__ hay __Apache__ nên trong một hệ thống hoạt động bình thường thì các câu lệnh này sẽ không cần thiết.

<br>

* Test:
Load trang web hoặc truy cập vào tài nguyên bất kì của trang web:

1. `F12`
2. Chọn `Network`(hay Mạng).
3. Reload lại
Khi đó người dùng sẽ thấy các request và kết quả trả về và khi chọn vào thì có thể xem header và tìm kiếm header `x-handled-by` xem file đó được xử lý bới __Nginx__ hay __Apache__.
<img width="1672" height="1008" alt="Image" src="https://github.com/user-attachments/assets/90745ab2-c69c-4588-ac25-f225a2ad3b95" />
<img width="1671" height="1010" alt="Image" src="https://github.com/user-attachments/assets/e4a1ece7-830d-4f55-8737-b59314169578" />  

<br>

Hoặc có thể dùng lệnh "curl" trên CMD thì khi đó kết quả sẽ trả về bao gồm cả __Header__ đã được cấu hình thêm.  

<img width="952" height="232" alt="Image" src="https://github.com/user-attachments/assets/55729419-39c0-405b-b8d4-9dfbad9db3ea" />  

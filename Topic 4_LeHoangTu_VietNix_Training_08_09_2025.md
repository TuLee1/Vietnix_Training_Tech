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
<img width="675" height="444" alt="Image" src="https://github.com/user-attachments/assets/2c9e7ad7-d99e-4841-9a0c-e0a87c367c57" />
<br>



* __Laravel__
<img width="618" height="491" alt="Image" src="https://github.com/user-attachments/assets/739c8b42-82c3-4102-ac5f-2ae492f32ada" />
<br>
<br>

* Ở 2 file này các `proxy_` chính là những gì người dùng cấu hình để __Nginx__ trở thành __Reverse Proxy__.
Trong các __proxy__ này thì dòng `proxy_pass`: sẽ trỏ request từ bên ngoài đến địa chỉ cần đến back end. Việc này giúp Apache chỉ cần xử lý các request ở port 443 -> port 8081  
* Ngoài ra nếu người dùng không sử dụng tùy chọn `return 301 ...` (chuyển request HTTP tự động sang HTTPS) thì có thể cấu hình tương tự của server 443 để các request từ port 80 của nginx sẽ đẩy về port 8080 tương ứng của apache.
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

### 4. SSL
Người dùng sử dụng lại SSL đã được ZeroSSL cấp trước đó. Về cách cấu hình thì ở phần __Apache__ và __Nginx__, người dùng có thể thấy các cấu hình liên quan đến ssl. Sau khi cài thành công thì SSL cũng đã áp dụng thành công lên 2 domain.


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

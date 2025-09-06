# NỘI DUNG TÌM HIỂU 05/09/2025
<br>




## XÂY DỰNG MÔ HÌNH LEMP, WEBSITE WORDPRESS + LARAVEL
### LEMP
#### Lý thuyết:
LEMP là một trong các giải pháp phổ biến để triển khai webserver bao gồm __L__ (Linux), __E__(Nginx), __M__ (MySQL hoặc MariaDB) và __P__ (PHP)




#### Triển khai:
##### Linux (L):
Trong bài viết này __LEMP__ được triển khai trên __VPS__ với hệ điều hành __Linux__, tuy nhiên người dùng có thể triển khai trên các hệ điều hành __Linux__ và liên quan như __CentOS, Ubuntu, ...__
<br>


##### Nginx (E):
Người dùng tiến hành cài Nginx trên thiết bị:
```
apt install nginx
```
Sau khi cài thành công thì nginx cần được bật lên:
```
sudo systemctl enable nginx
sudo systemctl start nginx
```
Kiểm tra trạng thái để chắc chắn nginx đã hoạt động:
```
sudo systemctl nginx status
```


```
root@tule-training-0509:~# sudo systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
    Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
    Active: active (running) since Fri 2025-09-05 09:30:30 +07; 1min 1s ago
      Docs: man:nginx(8)
  Main PID: 15972 (nginx)
     Tasks: 7 (limit: 11924)
    Memory: 8.3M
       CPU: 34ms
```
Nếu kết quả trả về của service là __active (running)__, khi đó dịch vụ đã hoạt động.


##### MariaDB (M):
Tiến hành cài __MariaDB__ lên VPS:
```
sudo apt install mariadb-server mariadb-client
```


Sau khi cài đặt, người dùng cần kiểm tra xem __MariaDB__ đã được bật chưa
```
systemctl status mariadb
```


```
root@tule-training-0509:~# systemctl status mariadb
● mariadb.service - MariaDB 10.6.22 database server
    Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: enabled)
    Active: active (running) since Fri 2025-09-05 09:34:22 +07; 1min 0s ago
      Docs: man:mariadbd(8)
            https://mariadb.com/kb/en/library/systemd/
   Process: 16940 ExecStartPre=/usr/bin/install -m 755 -o mysql -g root -d /var/run/mysqld (code=exited, status=0/SUCCESS)
   Process: 16941 ExecStartPre=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
   Process: 16943 ExecStartPre=/bin/sh -c [ ! -e /usr/bin/galera_recovery ] && VAR= ||   VAR=`/usr/bin/galera_recovery`; [ $? -eq 0 ]   && systemctl set-environment _WSREP_START_POSITION=$VAR || exit 1 (code=exited, status=0/SUCCESS)
   Process: 16983 ExecStartPost=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exited, status=0/SUCCESS)
   Process: 16985 ExecStartPost=/etc/mysql/debian-start (code=exited, status=0/SUCCESS)
  Main PID: 16972 (mariadbd)
    Status: "Taking your SQL requests now..."
     Tasks: 9 (limit: 78703)
    Memory: 61.6M
       CPU: 272ms
    CGroup: /system.slice/mariadb.service
            └─16972 /usr/sbin/mariadbd
```
Nếu trạng thái trả về không phải là __active (running)__ thì tiến hành khởi động dịch vụ:
```
sudo systemctl start mariadb
sudo systemctl enable mariadb
```
Việc __enable__ sẽ giúp dịch vụ tự khởi động khi thiết bị khởi động - người dùng có thể bỏ qua nếu không cần thiết


Sau khi cài đặt và khởi chạy, tiến hành chạy script để tăng cường tính bảo mật cho database về sau
```
sudo mysql_secure_installation
```
Tiến hành đặt __password__ (bắt buộc), và bật tắt các chức năng phù hợp với nhu cầu. 
__*Ví dụ dưới đây mang tính chất tham khảo và không phù hợp với tất cả trường hợp và nhu cầu sử dụng database*__
```
root@tule-training-0509:~# sudo mysql_secure_installation


NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
     SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!


In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.


Enter current password for root (enter for none):
OK, successfully used password, moving on...


Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.


You already have your root account protected, so you can safely answer 'n'.


Switch to unix_socket authentication [Y/n] n
... skipping.


You already have your root account protected, so you can safely answer 'n'.


Change the root password? [Y/n] n
... skipping.


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.


Remove anonymous users? [Y/n]
... Success!


Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.


Disallow root login remotely? [Y/n]
... Success!


By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.


Remove test database and access to it? [Y/n]
- Dropping test database...
... Success!
- Removing privileges on test database...
... Success!


Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.


Reload privilege tables now? [Y/n]
... Success!


Cleaning up...


All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.


Thanks for using MariaDB!
```
Sau khi đã thiết lập xong bạn có thể truy cập vào database với __root user__
```
sudo mariadb -u root
```
Nếu kết quả trả về tương tự dưới đây thì MariaDB đã hoạt động và có thể sử dụng:
```
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 37
Server version: 10.6.22-MariaDB-0ubuntu0.22.04.1 Ubuntu 22.04


Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.


Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```


##### PHP (P):
Người dùng tiến hành cài __PHP__ cho VPS với phiên bản phù hợp (phiên bản được sử dụng trong bài này là 8.1)
```
sudo apt install php php-fpm php-mysql php-cli php-common php-opcache php-readline php-mbstring php-xml php-gd php-curl -y
```


Kiểm tra trạng thái khởi chạy của __PHP__. Tương tự như các dịch vụ khác thì kết quả trả về tốt nhất sẽ là __active (running)__
```
systemctl status php8.1-fpm
```
```
● php8.1-fpm.service - The PHP 8.1 FastCGI Process Manager
    Loaded: loaded (/lib/systemd/system/php8.1-fpm.service; enabled; vendor preset: enabled)
    Active: active (running) since Fri 2025-09-05 09:39:35 +07; 1min 54s ago
      Docs: man:php-fpm8.1(8)
   Process: 30941 ExecStartPost=/usr/lib/php/php-fpm-socket-helper install /run/php/php-fpm.sock /etc/php/8.1/fpm/pool.d/www.conf 81 (code=exited, status=0/SUCCESS)
  Main PID: 30938 (php-fpm8.1)
    Status: "Processes active: 0, idle: 2, Requests: 0, slow: 0, Traffic: 0req/sec"
     Tasks: 3 (limit: 11924)
    Memory: 9.5M
       CPU: 35ms
    CGroup: /system.slice/php8.1-fpm.service
            ├─30938 "php-fpm: master process (/etc/php/8.1/fpm/php-fpm.conf)" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
            ├─30939 "php-fpm: pool www" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
            └─30940 "php-fpm: pool www" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" "" ""
```
<br>


---


#### Cấu hình để khởi chạy website WordPress và Laravel
* Cấu hình cho Domain __laravel.tule.vietnix.tech__:
```
sudo nano /etc/nginx/sites-available/laravel.tule.vietnix.tech
```
Nội dung file cấu hình có thể tham khảo nội dung cơ bản dưới đây:
```
server {
   listen 80;
   server_name laravel.tule.vietnix.tech;


   root /var/www/laravel/public;
   index index.php index.html index.htm;


   location / {
       try_files $uri $uri/ /index.php?$query_string;
   }


   location ~ \.php$ {
       include snippets/fastcgi-php.conf;
       fastcgi_pass unix:/run/php/php8.1-fpm.sock;
   }


   location ~ /\.ht {
       deny all;
   }
}
```
Trong đó:
* `root`: Đường dẫn đến file public (thư mục gốc) của Laravel.
* `fastcgi_pass unix:/run/php/php8.1-fpm.sock` : PHP-FPM socket, thay cho module của Apache vì trong bài viết này chỉ sử dụng Nginx nên sẽ không có Apache


Sau đó người dùng, tiến hành kích hoạch site và reload lại Nginx
```
sudo ln -s /etc/nginx/sites-available/laravel.tule.vietnix.tech /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
<br>

<img width="570" height="213" alt="Image" src="https://github.com/user-attachments/assets/85a1aceb-4353-49e2-985b-8dde2646e1ba" />

Sau khi reload thành công người dùng có thể kiểm thử bằng file info.php
```
sudo nano /var/www/laravel/info.php
```
Với nội dung :
```
<?php
phpinfo();
```
Sau đó truy cập vào đường dẫn `http://laravel.tule.vietnix.tech/info.php`  
<br>
<img width="1671" height="586" alt="Image" src="https://github.com/user-attachments/assets/5ac29c19-9de6-46aa-b15c-5caf3eaf726b" />  
<br>

Nếu giao diện như trên hình thì chứng tỏ website laravel đã hoạt động.


* Cấu hình cho Domain __wp.tule.vietnix.tech__:
Tạo thư mục cho __WordPress__ trên __LEMP__:
```
sudo mkdir -p /var/www/wordpress
cd /var/www/wordpress
```


Tải và giải nén __WorldPress__:
```
wget https://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz --strip-components=1
rm latest.tar.gz
```
Cấp quyền:
```
sudo chown -R www-data:www-data /var/www/wordpress
sudo chmod -R 755 /var/www/wordpress
```
Tạo database cho __WordPress__:
```
sudo mysql -u root -p
```


Sau đó truy cập vào MariaDB để tiến hành tạo database cho __WordPress__:
```
CREATE DATABASE wordpress_db;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'StrongPassword123!';
GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wp_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
Tiến hành tạo file cấu hình Nginx cho __WordPress__: 
Tạo file mới
```
sudo nano /etc/nginx/sites-available/wp.tule.vietnix.tech
```
Nội dung file cấu hình (tham khảo):
```
server {
   listen 80;
   server_name wp.tule.vietnix.tech;


   root /var/www/wordpress;
   index index.php index.html index.htm;


   location / {
       try_files $uri $uri/ /index.php?$args;
   }


   location ~ \.php$ {
       include snippets/fastcgi-php.conf;
       fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;  # chỉnh theo version PHP bạn dùng
       fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
       include fastcgi_params;
   }


   location ~ /\.ht {
       deny all;
   }
}
```
File cấu hình của __WordPress__ sẽ có những điểm tương đồng như khi người dùng cấu hình __Laravel__


Giống với khi cấu hình Laravel thì sau khi hoàn thành file cấu hình, người dùng tiến hành kích hoạt để Website __WordPress__ hoạt động:
```
sudo ln -s /etc/nginx/sites-available/wp.tule.vietnix.tech /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
Sau đó người dùng có thể truy cập vào đường dẫn `wp.tule.vietnix.tech` và giao diện của WordPress sẽ hiện ra. Sau khi người dùng đăng nhập vào và tiến hành tùy chỉnh thì khi tải lại giao diện của web sẽ hiện đúng như những gì đã được cài đặt, ví dụ như hình dưới:
<br>

<img width="1688" height="993" alt="Image" src="https://github.com/user-attachments/assets/2c435bef-7d77-4c47-bce3-219bb804e99d" />  
<br>


---
## CÀI SSL CHO 2 DOMAIN VỚI
Có nhiều đơn vị khác nhau cung cấp chứng chỉ SSL: trong bài viết này __ZeroSSL__ là đơn vị được sử dụng
### SSL cho WordPress:
Người dùng tiến hành truy cập vào trang chủ của __ZeroSSL__ tạo tài khoản (miễn phí)
Sau khi tạo ở giao diện chính người dùng tiến hành chọn __New Certificate__
Người dùng tiến hành điền các thông tin như __Domain__, __Validity__, ... trong bài viết này các tùy chọn __Pro__ sẽ không được sử dụng mà sẽ là các tùy chọn __miễn phí__.
__Sau khi xong bước đầu, người dùng cần tiến hành xác thực để chứng minh quyền sở hữu tên miền.__
* Người dùng tiến hành tải __file xác thực__ tại __Download Auth File__ (File được tải về dưới dạng .txt). Sau đó giải nén và upload file lên VPS (có thể sử dụng FTP)
  Ở trang của __ZeroSSL__ sẽ có hướng dẫn người dùng cần upload file xác nhận vào đường dẫn nào. Ví dụ như trong hình
  <br>
  
  <img width="1015" height="953" alt="Image" src="https://github.com/user-attachments/assets/5c741b9d-a69b-4fdc-83fc-8b18ab312c80" />
  <br>
  
  ```
  Upload the Auth File to your HTTP server under: /.well-known/pki-validation/
  ```
  Lưu ý đường dẫn `/.well-known/pki-validation/` có thể thay đổi theo thời gian nên người dùng cần dựa vào đường dẫn được cung cấp của SSL để có thể xác thực thành công
  Sau đó người dùng sẽ đưa file đó vào đường dẫn của Domain và phần mở rộng theo hướng dẫn trên.
  Sau khi hoàn thành sẽ cần cấp thêm quyền để nginx có thể truy cập vào: `-R 755`. Ngoài ra cần cấu hình thêm ở file nginx của Laravel bằng cách thêm khối này vào trong file:
  ```
  location ^~ /.well-known/ {
    allow all;
  }
Việc thêm khối này sẽ giúp ZeroSSL truy cập vào file Auth để xác thực và cập nhật.
Sau khi hoàn thành người dùng sẽ quay lại xác thực trên ZeroSSL và tải về file .zip gồm có: `ca_bundle.crt`, `certificate.crt`, `private.key`. Bởi __Nginx__ chỉ cần __1__ file `.crt` duy nhất nên người dùng cần gộp 2 file lại thành 1 file duy nhất.
```
cat zerossl_cert/ca_bundle.crt >> zerossl_cert/certificate.crt
```
Tạo file để lưu trữ file SSl trong Domain và để Nginx truy cập và áp dụng cho website của người dùng.
```
sudo mkdir -p /etc/ssl/zerossl/wp
```
Sau đó người dùng sẽ copy 2 file vào đường dẫn trên: file `.crt` (sau khi đã gộp 2 file) và `private.key`
Sau đó người dùng cần cấu hình file config của __Nginx__ để nó có thể áp dụng SSL cho trang web. Người dùng sẽ thêm các nội dung dưới đây.
```
ssl_certificate      /etc/ssl/zerossl/laravel/certificate.crt;
ssl_certificate_key  /etc/ssl/zerossl/laravel/private.key;
```
Các option này để __Nginx__ truy cập và áp dụng SSL
Ngoài ra để tăng tính bảo mật người dùng có thể chuyển từ __HTTP__ sang __HTTPS__ bằng cách thêm phần này vào cuối file
```
server {
    listen 80;
    server_name wp.tule.vietnix.tech;
    return 301 https://$host$request_uri;
}
```
Việc chuyển từ HTTP sang HTTPS sẽ giúp tăng tính bảo mật cho trang web của người dùng và thêm nhiều lợi ích khác nhưng sẽ không được đề cập ở bài viết này.

* Sau khi cấu hình, người dùng sẽ cần reload lại nginx. Nếu SSL được cài đặt đúng thì khi truy cập lại trang web sẽ có biểu tượng ổ khóa thay vì dòng chữ __không an toàn__ trước đó. Và khi xem thêm thì các thông tin của certificate và nhà cung cấp sẽ hiện ra và nếu đó là ZeroSSL thì có nghĩa SSL đã được thành công

### SSL cho Laravel:
Tương tự như khi bạn cấu hình cho WordPress, chỉ khác về đường dẫn trong việc tạo và lưu file. Người dùng có thể tham khảo và thực hiện tương tự


## TẠO TÀI KHOẢN FTP
Cài đặt _vsftpd (FTP server):
```
sudo apt install vsftpd -y
```
* Cấu hình vsftpd cơ bản
Mở file cấu hình
```
sudo nano /etc/vsftpd.conf
```
Cần thay đổi một số thông tin trong file `.conf` :
* `listen=YES`  
* `listen_ipv6=NO`: tắt chức năng sử dụng IPv6 cho việc sử dụng FTP  
* `anonymous_enable=NO`: không cho phép đăng nhập ẩn danh  
* `local_enable=YES`: cho phép user hệ thống login  
* `write_enable=YES`: Cho phép upload, chỉnh sửa file  
* `chroot_local_user=YES`: User bị giới hạn trong thư mục home của mình

Sau đó lưu file và thoát ra và khởi động lại dịch vụ
```
sudo systemctl restart vsftpd
sudo systemctl enable vsftpd
```
### Tạo user FTP cho từng dommain
#### User cho WordPress
```
sudo adduser ftp_wp
sudo usermod -d /var/www/wp.tule.vietnix.tech ftp_wp
sudo chown -R ftp_wp:ftp_wp /var/www/wp.tule.vietnix.tech
sudo chmod -R 755 /var/www/wp.tule.vietnix.tech
```
* `sudo adduser ftp_wp`: tạo user và tiến hành điền các thông tin để khởi tạo
* `sudo usermod -d /var/www/wp.tule.vietnix.tech ftp_wp`: chỉ định cho user truy cập vào nơi lưu trữ của domain (nếu người dùng không muốn user có thể truy cập vào home của VPS)
* Cấp quyền cho user với 2 lệnh còn lại.

#### User cho Laravel
```
sudo adduser ftp_laravel
sudo usermod -d /var/www/laravel.tule.vietnix.tech ftp_laravel
sudo chown -R ftp_laravel:ftp_laravel /var/www/laravel.tule.vietnix.tech
sudo chmod -R 755 /var/www/laravel.tule.vietnix.tech
```
các thông số và lệnh thì tương tự như user cho __WordPress__ chỉ khác về đường dẫn đến nơi lưu trữ của domain.


## Bật Remote MySQL (MariaDB) từ xa với user root
Người dùng kiểm tra lại trạng thái của hệ thống MySQL đang chạy
```
sudo systemctl status mysql
```
Nếu chưa được bật thì tiến hành bật MySQL.
```
sudo systemctl start mysql
```
Truy cập vào MySQL với quyền root
```
sudo mysql -u root -p
```
* Cấu hình cho phép root user truy cập từ xa
Kiểm tra user root hiện tại:
```
SELECT user, host FROM mysql.user;
```
Mặc định người dùng sẽ thấy root với tên "localhost"
Tạo bản sao root (hoặc chỉnh sửa trên chính root hiện tại) để truy cập remote:
```
CREATE USER 'root'@'%' IDENTIFIED BY 'MẬT_KHẨU_ROOT';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
`%`: cho phép root kết nối từ mọi IP
* Điều chỉnh file `.cnf` trong MySQL để truy cập được từ tất cả IP
Mở file cấu hình MySQL
```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```
Tìm tùy chọn `bind-address = 127.0.0.1` thay địa chỉ thành `0.0.0.0`: việc này sẽ giúp MySQL cho phép login từ tất cả IP
Cuối cùng khởi động lại MySQL để áp dụng những thay đổi vừa thực hiện:
```
sudo systemctl restart mysql
```

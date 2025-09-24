# VESTACP
Người dùng truy cập vào trang chủ của VestaCP `https://vestacp.com/install/` để cài đặt. Ở trang chủ người dùng sẽ chọn và điền các thông tin như WEB, FTP sau đó chọn __Generate Install Command__ . Trang chủ sẽ dựa vào đó và tạo sẵn các câu lệnh cho người dùng cài đặt.  
<img width="1342" height="694" alt="Image" src="https://github.com/user-attachments/assets/a1dd02da-a671-4e42-bbda-4e7dc5fb09d6" />  
<br><br>

<img width="1273" height="426" alt="Image" src="https://github.com/user-attachments/assets/c5a16140-324b-45f8-86c0-34ce4fa74c9f" />
<br><br>
Sau đó người dùng tiến hành cài đặt lên VPS với các câu lệnh được cung cấp

## Tạo Domain
Sau khi cài đặt người dùng sẽ được cung cấp đường dẫn và thông tin để đăng nhập vào giao diện của __Vestacp__  
<img width="1038" height="943" alt="Image" src="https://github.com/user-attachments/assets/84dc4feb-491d-4eba-a47d-14145763d9bd" />  
<br><br>

Người dùng tiến hành tạo domain mới và điền thông tin  

<img width="1920" height="1032" alt="Image" src="https://github.com/user-attachments/assets/c6324f12-5d02-42a1-b70d-dc514cac4256" />
<br><br>
<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/f93cae77-1d3a-4b11-b268-cddc40bff932" />  
<br><br>
Sau đó người dùng có thể gán SSL của trang web nếu có thì sau khi xong SSL sẽ được gán vào. Nếu chưa có người dùng có thể tạo SSL free trên Vesta bằng `Generate CSR` và điền thông tin và sau 5 phút thì ssl sẽ được tạo nhưng do đây là ssl tự tạo nên sẽ có thông báo "Không an toàn" khi truy cập  

<img width="1920" height="932" alt="Image" src="https://github.com/user-attachments/assets/d1da8ded-152e-4493-b42a-185be94a65f5" />
<br><br>

## Upload source code
Khi tạo tài khoản người dùng sẽ tạo cả FTP để upload source code nếu chưa tạo người dùng có thể vào domain và tạo. Sau đó người dùng tiến hành upload source code cho cả WP và Laravel  
<img width="1920" height="998" alt="Image" src="https://github.com/user-attachments/assets/d24b64bf-a156-4bfc-958b-57e781c9570f" />
<br><br>

## Upload Database và Import
Người dùng tiến hành tạo Database trên VestaCP. Lưu ý là tên của db và username sẽ mặc định có tiền tố `admin_` ở phía trước  
<img width="1915" height="995" alt="Image" src="https://github.com/user-attachments/assets/aac9bdb9-7963-476e-aa9b-31c6d6aaf44b" />
<br><br>
Sau khi đã add thì người dùng vào __PHPMyAdmin__ và Import dữ liệu  

## Cấu hình để Laravel và WordPress kết nối vào DB vừa được tạo
Người dùng sẽ cấu hình như khi triển khai ở VPS cho file config ở 2 trang web để kết nối được đến với DB ở file .env (laravel) và wp-config (ở trang wp)  
<img width="730" height="470" alt="Image" src="https://github.com/user-attachments/assets/c0934270-763e-4a1e-bbb1-260933cac4b2" />
<br><br>
<img width="383" height="905" alt="Image" src="https://github.com/user-attachments/assets/93b3a59e-b082-4631-ab7e-d8823159803e" />
<br><br>
Sau đó reload thì các trang web sẽ hoạt động


## SSL
Khi tạo domain thì người dùng đã có thể thêm phần SSL cho trang web như trên. Trong trường hợp người dùng cần chỉnh sửa thì nếu không muốn sử dụng giao diện web thì người dùng có thể tìm được đường dẫn của các file key hay crt trong file config của WordPress và Laravel  
<img width="921" height="759" alt="Image" src="https://github.com/user-attachments/assets/752e53e1-549d-4f98-91c1-b6a4e49fbe5a" />
<br><br>
<img width="1692" height="109" alt="Image" src="https://github.com/user-attachments/assets/f422139f-287c-4fc6-beb9-af10937a7107" />
<br><br>
## Lưu ý với trang Laraveel
* Do Vesta mặc định sẽ trỏ domain vào folder `public_html` trong khi Laravel thì cần phải trỏ vào folder `public`. Thì để cho trang web hoạt động thì sẽ có 2 cách
    * Trỏ lại Root vào folder `public` của Laravel và cấu hình lại root trong file conf
<img width="1128" height="776" alt="Image" src="https://github.com/user-attachments/assets/1c1d918b-bec2-4a34-9b08-a67262828af6" />
      

    * Upload các file trong folder `public` vào folder `public_html` và các source còn lại nằm ở ngoài: nếu người dùng chọn cách này thì sẽ cần config lại open_basedir của php để laravel có quyền đọc các file source nằm ngoài public_html
  Dù chọn cách nào thì cũng sẽ cần can thiệp vào file config nhưng cách 1 sẽ dễ dàng hơn và hiệu quả hơn.
* Do vestacp hỗ trợ tối đa ubuntu 18 và ubuntu 18 thì sẽ sử dụng php7.2, vì vậy nếu source Laravel của người dùng yêu cầu cao hơn (>= 8.1) thì sẽ cần phải cài thêm. Do Ubuntu 18 là bản cũ nên cài bằng gói apt sẽ không thể và người dùng cần cài 1 cách thủ công
```
# Cài thủ công php 8.1
cd /usr/local/src
sudo wget https://www.php.net/distributions/php-8.1.21.tar.gz
sudo tar xvf php-8.1.21.tar.gz
cd php-8.1.21

# Biên dịch thủ công PHP và FPM
sudo ./configure \
--prefix=/usr/local/php81 \
--enable-fpm \
--with-fpm-user=www-data \
--with-fpm-group=www-data \
--enable-mbstring \
--with-curl \
--with-openssl \
--with-zlib \
--enable-soap \
--enable-xml \
--with-mysqli \
--with-pdo-mysql \
--enable-bcmath \
--enable-intl \
--with-gd

# Compile và cài đặt
sudo make -j$(nproc)
sudo make install

# Tạo file config FPM
sudo cp /usr/local/php81/etc/php-fpm.conf.default /usr/local/php81/etc/php-fpm.conf
sudo cp /usr/local/php81/etc/php-fpm.d/www.conf.default /usr/local/php81/etc/php-fpm.d/www.conf

sudo nano /etc/systemd/system/php81-fpm.service

#Nội dung file
[Unit]
Description=The PHP 8.1 FastCGI Process Manager
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/php81/sbin/php-fpm --nodaemonize --fpm-config /usr/local/php81/etc/php-fpm.conf
Restart=always

[Install]
WantedBy=multi-user.target

```

Sau khi đã cài thì người dùng cần cấu hình trong config apache của trang Laravel để Apache sẽ sử dụng php8.1 thay vì 7.2  
<img width="908" height="778" alt="Image" src="https://github.com/user-attachments/assets/63a13dd6-e09e-46a5-899e-c6b6eac88bad" />


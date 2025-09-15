# AAPANEL
Cài Aapanel lên VPS:
```
wget -O install.sh http://www.aapanel.com/script/install-ubuntu_6.0_en.sh && sudo bash install.sh
```
Sau khi cài thì __aapanel__ sẽ cung cấp thông tin gồm đường link để truy cập giao diện web, user và password để đăng nhập
hình

## Upload source code
Người dùng tiến hành truy cập và đăng nhập giao diện web
Khi mới vào lần đầu, sẽ có yêu cầu đề cử để người dùng cài với 2 dịch vụ ở đây sẽ được đề cử là __LNMP (LEMP)__ và __LAMP__. Người dùng tiến hành chọn mô hình phù hợp, ngoài ra có thể chọn thêm các dịch vụ cần thiết và chọn __One-click__
hình
hình

## Tạo Domain và trỏ về source code
Người dùng tiến hành tạo domain và cấu hình (việc thao tác cho 2 domain là tương tự nhau chỉ có lưu ý nhỏ về đường dẫn ở laravel là domain cần trỏ vào phần public để có thể load được trang web đúng như mong muốn)

* Điền `Domain name`
* Trỏ domain về file source code người dùng vừa upload lên
* FTP (tùy chọn)
* Tạo database - để về sau có thể import database gốc ở VPS lên


## Import data
Sau khi đã tạo database, người dùng tiến hành vào phpmyadmin để import file sql cho website
hình

Việc import tương tự như khi người dùng thao tác import bình thường trên phpmyadmin
hình  
Sau khi import xong các dữ liệu sẽ hiện ở bên trái - có nghĩa dữ liệu đã được chuyển vào

### Cấu hình file config để 2 domain trỏ về đúng database tương ứng
Sau khi database của aapanel đã được tạo và import dữ liệu người dùng sẽ chỉnh sửa các thông tin về `db_name`, `db_username`, `db_password` trên cả 2 source code như khi người dùng thực hiện trên VPS
hình
hình

* Trong bài viết này, Mysql được cấu hình sử dụng port 3307 (thay vì mặc định 3306) vì để tránh xung đột với các dịch vụ khác - nên khi cấu hình các dịch vụ liên quan đến mysql thì người dùng cần lưu ý (nếu người dùng sử dụng port mặc định thì sẽ không ảnh hưởng)


## Cài SSL
Người dùng sẽ sử dụng lại các file ssl trước đó đã tạo cho 2 domain và upload lên aapanel 
hình
Ở giao diện này người dùng sẽ upload nội dung file `.key` và `.crt`  
Ở file `.crt` nếu người dùng có 2 file .crt thì sẽ up file ca_bundle.crt và sau đó mới dán nội dung của file certificate.crt vào. Nếu thực hiện đúng thì sau khi save ssl sẽ được bật đồng thời người dùng còn sẽ biết hạn của chứng chỉ ssl còn bao lâu như trên hình

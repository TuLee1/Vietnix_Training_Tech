# AAPANEL
Cài Aapanel lên VPS:
```
wget -O install.sh http://www.aapanel.com/script/install-ubuntu_6.0_en.sh && sudo bash install.sh
```
Sau khi cài thì __aapanel__ sẽ cung cấp thông tin gồm đường link để truy cập giao diện web, user và password để đăng nhập
<img width="1011" height="213" alt="Image" src="https://github.com/user-attachments/assets/5e7ecb2f-f625-4cdc-b738-977884c9a822" />

<br><br>

## Upload source code
Người dùng tiến hành truy cập và đăng nhập giao diện web
Khi mới vào lần đầu, sẽ có yêu cầu đề cử để người dùng cài với 2 dịch vụ ở đây sẽ được đề cử là __LNMP (LEMP)__ và __LAMP__. Người dùng tiến hành chọn mô hình phù hợp, ngoài ra có thể chọn thêm các dịch vụ cần thiết và chọn __One-click__
<img width="1650" height="999" alt="Image" src="https://github.com/user-attachments/assets/37001a5c-117a-4cba-a680-a9ba6f3bb799" />

<br><br>

<img width="704" height="600" alt="Image" src="https://github.com/user-attachments/assets/551fd332-77d1-49ca-8152-e02c5058f33b" />  

Có 2 cách upload source code : 
* Upload source code bằng giao diện web
* Copy vào đường dẫn của aapanel sử dụng cmd: `/www/wwwroot/` 

## Tạo Domain và trỏ về source code
Người dùng tiến hành tạo domain và cấu hình (việc thao tác cho 2 domain là tương tự nhau chỉ có lưu ý nhỏ về đường dẫn ở laravel là domain cần trỏ vào phần public để có thể load được trang web đúng như mong muốn)
<img width="1918" height="1003" alt="Image" src="https://github.com/user-attachments/assets/ff0d791d-e2fd-4a6b-b091-3dbb09cacd17" />

<br><br>

<img width="863" height="407" alt="Image" src="https://github.com/user-attachments/assets/237eb989-828f-4aa6-b619-6d46d2616d8f" />  

<br><br>

Nếu người dùng triển khai web sử dụng laravel thì có thể tham khảo như hình  

* Điền `Domain name`
* Trỏ domain về file source code người dùng vừa upload lên
* FTP (tùy chọn)
* Tạo database - để về sau có thể import database gốc ở VPS lên


## Import data
Sau khi đã tạo database, người dùng tiến hành vào phpmyadmin để import file sql cho website
<img width="1919" height="1000" alt="Image" src="https://github.com/user-attachments/assets/16bb7caf-0c22-4c3d-b7ad-7a4c382b0244" />  

<br><br>

Việc import tương tự như khi người dùng thao tác import bình thường trên phpmyadmin
<img width="1919" height="1003" alt="Image" src="https://github.com/user-attachments/assets/3a36dc35-5709-4114-b1f7-66f5df42521a" />  
Sau khi import xong các dữ liệu sẽ hiện ở bên trái - có nghĩa dữ liệu đã được chuyển vào

### Cấu hình file config để 2 domain trỏ về đúng database tương ứng
Sau khi database của aapanel đã được tạo và import dữ liệu người dùng sẽ chỉnh sửa các thông tin về `db_name`, `db_username`, `db_password` trên cả 2 source code như khi người dùng thực hiện trên VPS
<img width="1536" height="702" alt="Image" src="https://github.com/user-attachments/assets/a9432fbf-6be7-4a23-84be-67eba6028fb1" />  

<br><br>

<img width="1544" height="706" alt="Image" src="https://github.com/user-attachments/assets/8fd56019-f1dd-42de-b437-213baaa45363" />  

<br><br>

* Trong bài viết này, Mysql được cấu hình sử dụng port 3307 (thay vì mặc định 3306) vì để tránh xung đột với các dịch vụ khác - nên khi cấu hình các dịch vụ liên quan đến mysql thì người dùng cần lưu ý (nếu người dùng sử dụng port mặc định thì sẽ không ảnh hưởng)


## Cài plugins trên WordPress
Để cài Plugin người dùng cần đăng nhập vào giao diện Admin của web WordPress. Cách truy cập là sử dụng `/wp-admin` ví dụ như domain đang được sử dụng ở đây `wp.tule.vietnix.tech` thì để đăng nhập vào admin thì sẽ là `wp.tule.vietnix.tech/wp-admin`  

<img width="1920" height="1003" alt="Image" src="https://github.com/user-attachments/assets/3db50954-22d0-445e-9865-f775dcd0d432" />  

Có 2 cách để cài plugin: cài trực tiếp trên với các Plugin có sẵn trên WordPress hoặc tải plugins dưới dạng Zip và upload lên để cài đặt.  

Cách 1 - Cài trực tiếp các Plugin được cung cấp trên giao diện Admin: 
* Chọn plugins
* Thêm plugin
* Chọn plugin phù hợp
* Cài đặt
<img width="1918" height="998" alt="Image" src="https://github.com/user-attachments/assets/278402ac-90be-4543-a6b0-1b9e26836e8f" />

<br><br>

Cách 2 - Cài bằng cách upload plugin (được nén thành file .zip) và upload lên để cài đặt: 
* Người dùng tải plugin và lưu file .zip
* Ở giao diện Plugin sẽ có phần  __Tải plugin lên__
* Người dùng upload plugin đã tải
* Cài đặt và kích hoạt plugin
<img width="1920" height="1006" alt="Image" src="https://github.com/user-attachments/assets/26748e52-c56c-48d2-8387-956b0cee4c4d" />

<img width="1919" height="995" alt="Image" src="https://github.com/user-attachments/assets/7b572f9a-ada4-4a58-ab62-5bdd0f90154c" />

<br><br>

Lưu ý: Sau khi cài đặt người dùng sẽ cần kích hoạt Plugin để có thể chạy được (áp dụng cho cả 2 cách cài)

### Plugin All-in-One WP Migration and Backup
Đây là Plugin giúp cho người dùng đóng gói tất cả những gì cần thiết cho một website WordPress lại thành 1 file (All-in-one). Việc này sẽ giúp người dùng chuyển đổi, lưu trữ, hay muốn backup dữ liệu cho website một cách dễ dàng vì tất cả đã toàn bộ đã được đóng gói lại thành 1 file với định dạng `.wpress`.
* Xuất: Người dùng có thể xuất trang web và đóng gói lại và plugin hỗ trợ để upload lên nhiều dịch vụ khác nhau như AWS, Google Cloud, ...  

<img width="1920" height="988" alt="Image" src="https://github.com/user-attachments/assets/3d8c0b98-be2e-4c52-a2cd-a4e558ace3d6" />  
<img width="548" height="208" alt="Image" src="https://github.com/user-attachments/assets/869d0a00-f6b9-4500-acd4-dacc47e83273" />

<br><br>

* Nhập: Người dùng sẽ dùng file `.wpress` để upload và plugin này có thể xử lý và sẽ deploy trang web cho người dùng.  
<img width="1919" height="992" alt="Image" src="https://github.com/user-attachments/assets/bc7aa07c-0f1b-4f5b-a3ff-943a9690f43f" />  


* Sao lưu: Ở đây sẽ có lưu trữ cả file `.wpress` được tạo ra bao gồm cả ở phần __Xuất__ và phần __Sao lưu__ để người dùng có nhu cầu tái sử dụng các phiên bản backup cũ hơn.
hình  
<img width="1920" height="988" alt="Image" src="https://github.com/user-attachments/assets/127d4821-cfdd-44c8-8a72-7f706c3739a9" />

<br><br>

* Ngoài ra plugins còn có các chức năng khác như: khôi phục dữ liệu, tạo lịch trình tự động cho việc backup, sao lưu dữ liệu định kì cho website

<img width="1920" height="998" alt="Image" src="https://github.com/user-attachments/assets/ed32bd12-ad84-470b-bb72-ecd3afa9a4c7" />  



### WP-Optimize-Cache vs LiteSpeed Cache
* WP-Optimize-Cache: một số chức năng chính
  * Optimize Database: plugins cung cấp các option để tối ưu database
  * Trực quan hóa dữ liệu như hình ảnh giúp dễ xử lý
  * Tạo cache cho website giúp load nhanh hơn
  <img width="1919" height="1002" alt="Image" src="https://github.com/user-attachments/assets/75cf7f91-393c-4ff0-9caf-341879c7e490" />

<br><br>

<img width="1920" height="995" alt="Image" src="https://github.com/user-attachments/assets/3c1544b2-c81b-4d86-b7ac-1a9546fb1562" />  

<br><br>

 
* LiteSpeed Cache: Một số chức năng chính
  * Tối ưu trang web (ảnh, css, ...) bằng __Quic.cloud
  * Tạo cache cho trang web và hỗ trợ cấu hình cache sâu hơn với nhiều option cho từng nhu cầu của người dùng
  * Tối ưu database (tương tự như WP-Optimize)
  * Tối ưu trong việc xử lý các file và yêu cầu gửi về
<img width="1920" height="997" alt="Image" src="https://github.com/user-attachments/assets/4baf9c09-c0ec-4130-9607-7f3c625f7369" />

<br><br>

<img width="1919" height="1002" alt="Image" src="https://github.com/user-attachments/assets/a783da13-4b5c-45bc-b87f-fc9d8b137744" />  

<br><br>
  
* Khi nào sử dụng LiteSpeed Cache: LiteSpeed Cache sẽ hiệu quả và dành cho các hệ thống
  * Đang sử dụng máy chủ LiteSpeed/OpenLiteSpeed để chạy Web hoặc sử dụng các dịch vụ hosting có hỗ trợ LiteSpeed (CPanel)
  * Người dùng có tối ưu hiệu năng với cache (mức độ Server) thay vì chỉ sử dụng cache ở PHP
  * Yêu cầu về khả năng xử lý và tiếp nhận yêu cầu với lưu lượng cao 
## Cài SSL
Người dùng sẽ sử dụng lại các file ssl trước đó đã tạo cho 2 domain và upload lên aapanel 
<img width="1919" height="1003" alt="Image" src="https://github.com/user-attachments/assets/0f2e3c51-8393-41ac-aef4-93644a784474" />  

Ở giao diện này người dùng sẽ upload nội dung file `.key` và `.crt`  
Ở file `.crt` nếu người dùng có 2 file .crt thì sẽ up file ca_bundle.crt và sau đó mới dán nội dung của file certificate.crt vào. Nếu thực hiện đúng thì sau khi save ssl sẽ được bật đồng thời người dùng còn sẽ biết hạn của chứng chỉ ssl còn bao lâu như trên hình

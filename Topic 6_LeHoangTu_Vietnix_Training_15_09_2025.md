# AAPANEL
Cài Aapanel lên VPS:
```
wget -O install.sh http://www.aapanel.com/script/install-ubuntu_6.0_en.sh && sudo bash install.sh
```
Sau khi cài thì __aapanel__ sẽ cung cấp thông tin gồm đường link để truy cập giao diện web, user và password để đăng nhập
<img width="1011" height="213" alt="Image" src="https://github.com/user-attachments/assets/5e7ecb2f-f625-4cdc-b738-977884c9a822" />

## Upload source code
Người dùng tiến hành truy cập và đăng nhập giao diện web
Khi mới vào lần đầu, sẽ có yêu cầu đề cử để người dùng cài với 2 dịch vụ ở đây sẽ được đề cử là __LNMP (LEMP)__ và __LAMP__. Người dùng tiến hành chọn mô hình phù hợp, ngoài ra có thể chọn thêm các dịch vụ cần thiết và chọn __One-click__
<img width="1650" height="999" alt="Image" src="https://github.com/user-attachments/assets/37001a5c-117a-4cba-a680-a9ba6f3bb799" />
<img width="704" height="600" alt="Image" src="https://github.com/user-attachments/assets/551fd332-77d1-49ca-8152-e02c5058f33b" />  
Có 2 cách upload source code : 
* Upload source code bằng giao diện web
* Copy vào đường dẫn của aapanel sử dụng cmd: `/www/wwwroot/` 

## Tạo Domain và trỏ về source code
Người dùng tiến hành tạo domain và cấu hình (việc thao tác cho 2 domain là tương tự nhau chỉ có lưu ý nhỏ về đường dẫn ở laravel là domain cần trỏ vào phần public để có thể load được trang web đúng như mong muốn)
<img width="1918" height="1003" alt="Image" src="https://github.com/user-attachments/assets/ff0d791d-e2fd-4a6b-b091-3dbb09cacd17" />
<img width="863" height="407" alt="Image" src="https://github.com/user-attachments/assets/237eb989-828f-4aa6-b619-6d46d2616d8f" />  

Nếu người dùng triển khai web sử dụng laravel thì có thể tham khảo như hình  

* Điền `Domain name`
* Trỏ domain về file source code người dùng vừa upload lên
* FTP (tùy chọn)
* Tạo database - để về sau có thể import database gốc ở VPS lên


## Import data
Sau khi đã tạo database, người dùng tiến hành vào phpmyadmin để import file sql cho website
<img width="1919" height="1000" alt="Image" src="https://github.com/user-attachments/assets/16bb7caf-0c22-4c3d-b7ad-7a4c382b0244" />  

Việc import tương tự như khi người dùng thao tác import bình thường trên phpmyadmin
<img width="1919" height="1003" alt="Image" src="https://github.com/user-attachments/assets/3a36dc35-5709-4114-b1f7-66f5df42521a" />  

Sau khi import xong các dữ liệu sẽ hiện ở bên trái - có nghĩa dữ liệu đã được chuyển vào

### Cấu hình file config để 2 domain trỏ về đúng database tương ứng
Sau khi database của aapanel đã được tạo và import dữ liệu người dùng sẽ chỉnh sửa các thông tin về `db_name`, `db_username`, `db_password` trên cả 2 source code như khi người dùng thực hiện trên VPS
<img width="1536" height="702" alt="Image" src="https://github.com/user-attachments/assets/a9432fbf-6be7-4a23-84be-67eba6028fb1" />  

<img width="1544" height="706" alt="Image" src="https://github.com/user-attachments/assets/8fd56019-f1dd-42de-b437-213baaa45363" />  


* Trong bài viết này, Mysql được cấu hình sử dụng port 3307 (thay vì mặc định 3306) vì để tránh xung đột với các dịch vụ khác - nên khi cấu hình các dịch vụ liên quan đến mysql thì người dùng cần lưu ý (nếu người dùng sử dụng port mặc định thì sẽ không ảnh hưởng)


## Cài SSL
Người dùng sẽ sử dụng lại các file ssl trước đó đã tạo cho 2 domain và upload lên aapanel 
<img width="1919" height="1003" alt="Image" src="https://github.com/user-attachments/assets/0f2e3c51-8393-41ac-aef4-93644a784474" />  

Ở giao diện này người dùng sẽ upload nội dung file `.key` và `.crt`  
Ở file `.crt` nếu người dùng có 2 file .crt thì sẽ up file ca_bundle.crt và sau đó mới dán nội dung của file certificate.crt vào. Nếu thực hiện đúng thì sau khi save ssl sẽ được bật đồng thời người dùng còn sẽ biết hạn của chứng chỉ ssl còn bao lâu như trên hình

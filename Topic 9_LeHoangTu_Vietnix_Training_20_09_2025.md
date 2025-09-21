# Window Server

## Allow, Block, Giới hạn IP/Port trên Window FW
### A. Allow/Block
Người dùng vào __Windows Firewall with Advanced Security__  

<img width="1877" height="969" alt="Image" src="https://github.com/user-attachments/assets/1b231c96-b4b0-4d01-aa16-123068bf5694" />
<img width="1920" height="1002" alt="Image" src="https://github.com/user-attachments/assets/bb7a5bce-03bd-4860-8a0e-aa5ab1fbcdbe" />
<img width="1920" height="1038" alt="Image" src="https://github.com/user-attachments/assets/6425a369-c592-47d1-b905-3891e244a9f2" />
<img width="1920" height="1030" alt="Image" src="https://github.com/user-attachments/assets/20ba75cd-7e23-4169-87f1-49f05d44a62a" />
<img width="1919" height="1037" alt="Image" src="https://github.com/user-attachments/assets/308439d7-7b9b-4ef4-9a16-734883289925" />
<br><br>

Với `IP` thì ở phần __Rule Type__ sẽ chọn custom và thực hiện như hình dưới  
<img width="1920" height="1038" alt="Image" src="https://github.com/user-attachments/assets/2d3c858b-51c9-48ea-bfe1-f1b089b382f0" />
<img width="1920" height="1041" alt="Image" src="https://github.com/user-attachments/assets/54885c96-dd60-41f5-852e-b0e9d12092ee" />
<img width="745" height="610" alt="Image" src="https://github.com/user-attachments/assets/906920ae-28c0-4349-87f4-7c0093073dd0" />
<br><br>

Với Block thì thực hiện tương tự nhưng sẽ đổi ở phần `Action` tick vào __Block Connection__

### B. Chỉ định ip truy cập vào port nhất định
Vẫn thực hiện tương tự như khi Allow IP. Ở Protocol sẽ chọn giao thức cho port (như UDP, TCP, ICMP, ...). Sau đó ở phần Scope sẽ điền như khi allow/block IP
<img width="732" height="595" alt="Image" src="https://github.com/user-attachments/assets/5f835aa6-54b3-4f98-a618-e43e472feaf1" />

## Cài đặt trang web WordPress
### Cài IIS
Người dùng tiến hành cài IIS trên windows server  

<img width="1880" height="959" alt="Image" src="https://github.com/user-attachments/assets/226ac32f-c4e8-4bb7-9b85-e9077d745aeb" />
<img width="1876" height="1009" alt="Image" src="https://github.com/user-attachments/assets/cbba43f3-8232-4760-8584-0a1ab7f5862f" />
<img width="1869" height="981" alt="Image" src="https://github.com/user-attachments/assets/5f8d269c-22c1-4d87-a0ad-616614bb35d7" />
<img width="1881" height="1012" alt="Image" src="https://github.com/user-attachments/assets/b855dd75-4ba2-425c-8182-40f22bfcf789" />
<img width="1875" height="1011" alt="Image" src="https://github.com/user-attachments/assets/7de445e9-bf32-4714-90b0-36d2a7a27c10" />

### Cài PHP
Người dùng tiến hành cài PHP để có thể chạy được các file php cho trang WordPress (Chọn file loại NTS). Tải source PHP ở trang chủ và giải nén. Sau đó sẽ trỏ PHP vào để xử lý các file của trang WordPress
Người dùng cần cài CGI để có thể gán file php-cgi và xử lý các file. Ở phần __IIS__ chọn __Tasks__ ở góc phải -> __Add Roles and Features Wizard và cài __CGI__ như hình  
<img width="1888" height="1009" alt="Image" src="https://github.com/user-attachments/assets/0b41b625-355d-4720-80fd-7cd7423a974e" />

Source code sẽ dùng source mặc định của WordPress (có thể tải tại trang chủ của WP). Sau đó truy cập vào __IIS Manager__, add Website và điền các thông tin cần thiết như Domain, Physical Path (trỏ vào source WP).  
Để trỏ PHP, người dùng sẽ vào phần Handler Mappings của trang WP -> Add Module Mapping. Ở phần Module chọn FastCGIModule và tìm file php-cgi
<img width="1792" height="960" alt="Image" src="https://github.com/user-attachments/assets/14b1924e-cf29-4955-9c0f-3477bedd4315" />
<br><br>
### Cài Mysql
Tải Mysql Installer từ trang chủ và tiến hành cài đặt

<img width="822" height="617" alt="Image" src="https://github.com/user-attachments/assets/1234e8f2-7c81-4978-8183-b7ef53d1d43b" />
<img width="869" height="631" alt="Image" src="https://github.com/user-attachments/assets/b1ddad00-6edd-40e7-be5d-0e1c5e693f7c" />
<img width="799" height="621" alt="Image" src="https://github.com/user-attachments/assets/492c5b2d-a94d-4c3a-a619-7edd87bcfd22" />  

Mysql sẽ yêu cầu cài thêm Visual C++  

<img width="582" height="393" alt="Image" src="https://github.com/user-attachments/assets/03136e9b-4878-4401-8c9c-5664a8fbb59b" />  
Sau khi đã có mysql người dùng sẽ tiến hành tạo db, và usename password và chỉnh sửa file config của wp để truy cập.

### Trỏ domain vào file index.php và gán SSL
Do IIS Manager mặc định sẽ trỏ domain vào file index.html mặc định nên người dùng cần sẽ thay đổi và ưu tiên trỏ vào index.php
Người dùng chọn Default Document, sau đó add file Index.php  

<img width="1910" height="939" alt="Image" src="https://github.com/user-attachments/assets/d15777e2-b4b5-4fb5-94d4-6c347b98538a" />    

Để gán SSL người dùng cần tạo file `.pfx` từ các file certificate. Người dùng có thể dùng OpenSSL hoặc tạo ở một số trang.  

<img width="1477" height="758" alt="Image" src="https://github.com/user-attachments/assets/19e0e43d-3f48-48a8-9c1b-d6b8d53a4732" />  
Sau đó người dùng nhấn chọn và sau đó nhập password đã được sử dụng để tạo ra file `.pfx` và import. Sau khi import thì người dùng sẽ bật https cho trang web ở phần __Bindings__ và add  

<img width="1878" height="947" alt="Image" src="https://github.com/user-attachments/assets/5102fa69-c0bc-49dd-9c9b-dbd0e64ae446" />

## SQL Server 2016
Người dùng tiến hành tải file ISO của SQL Server tại đường dẫn `https://software.vietnix.tech/datastore/sources/SQL_Server/sql2016/`. Sau đó Mount file Iso và chọn Setup để bắt đầu cài đặt  
<img width="1919" height="710" alt="Image" src="https://github.com/user-attachments/assets/ca56dfac-2d14-4bf2-bbb8-fbc52a5e9461" />
<img width="674" height="484" alt="Image" src="https://github.com/user-attachments/assets/b8d5a60e-5298-45c4-b9f6-9709a31c493d" />
<img width="1503" height="874" alt="Image" src="https://github.com/user-attachments/assets/0e2795fa-caae-4625-a56b-acc8ab3cfde9" />
<img width="1084" height="772" alt="Image" src="https://github.com/user-attachments/assets/392ade9c-1e5a-41d9-9a0f-31e5930df716" />
<img width="1709" height="874" alt="Image" src="https://github.com/user-attachments/assets/a719f8bf-3ca9-434f-8643-8531ff3f1734" />

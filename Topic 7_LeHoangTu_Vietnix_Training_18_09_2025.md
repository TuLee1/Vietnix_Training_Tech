# Cyber Panel
## Cài đặt Cpanel
Người dùng tiến hành cài đặt Cyber Panel và tùy chọn các lựa chọn phù hợp với nhu cầu
```
sh <(curl https://cyberpanel.net/install.sh || wget - O - https://cyberpanel.net/install.sh)
```
Sau khi cài đặt xong thì thông tin truy cập sẽ được cung cấp
<br><br>  
<img width="630" height="593" alt="Image" src="https://github.com/user-attachments/assets/957e9954-95f6-4d68-9ec2-1aa8711ee2d4" />
<br> <br>

## Deploy 2 website lên Cyber Panel
Sau khi đã có thông tin người dùng tiến hành truy cập vào giao diện Web và cấu hình để deploy trang web
### Tạo Package
Cyber Panel sẽ yêu cầu người dùng cần phải tạo package trước khi có thể tạo website cho domain (áp dụng cho cả 2 domain)
<br><br>
<img width="1916" height="998" alt="Image" src="https://github.com/user-attachments/assets/d4009f20-18b4-40c5-8f28-e93f17d0c9df" />  
<br> <br>
Người dùng tiến hành tạo package cho cả 2 domain

### Tạo website
Người dùng tiến hành tạo website trên giao diện  
<br> <br>

<img width="1920" height="1005" alt="Image" src="https://github.com/user-attachments/assets/5aa81790-95cd-459f-9dda-22c8ac2a8f04" />
<br><br>

Người dùng sẽ tạo dựa trên package được tạo trước đó và điền thông tin còn lại như `domain`, `email` (việc tạo trang web cho 2 domain là tương tự nhau)
<br> <br>

<img width="1920" height="1006" alt="Image" src="https://github.com/user-attachments/assets/531c1e03-f01d-43e6-8070-dce3c9f4b3e9" />

### Upload source code
Người dùng tiến hành vào phần File Manager và upload source code
Có 2 cách là Upload trực tiếp trên giao diện web UI hoặc sử dụng FTP. Người dùng sẽ upload trực tiếp vào file đã được cấu hình mặc định là `public_html`  
* Lưu ý cho website sử dụng Laravel: do website laravel cần trỏ domain vào file public nên người dùng sẽ cấu hình bằng cách truy cập vào `Manage` và sửa đổi đường dẫn Vhost như trong hình dưới
   <br> <br>
<img width="1919" height="995" alt="Image" src="https://github.com/user-attachments/assets/44eeae97-f022-48b9-b6dd-e304e9999194" />
<br><br>
### Tạo database và Import dữ liệu
Người dùng sẽ tạo database để có thể đưa dữ liệu của website vào
<br> <br>
<img width="1920" height="1000" alt="Image" src="https://github.com/user-attachments/assets/59cdb0e9-a7ea-4736-b42f-6cf804b30388" />
<br><br>

Sau khi tạo, người dùng sẽ chọn PHPMyAdmin để vào import dữ liệu. Thao tác import sẽ tương tự như những topic trước.  
Sau đó cấu hình trong các file .env (của laravel) và php-config (của wp) trỏ vào đúng database đã tạo
<br><br>
<img width="660" height="806" alt="Image" src="https://github.com/user-attachments/assets/c0dd9046-2b96-4861-a5ed-ec7cfc54bfd5" />
<br><br>
<img width="1920" height="1004" alt="Image" src="https://github.com/user-attachments/assets/d1791f27-f3c6-45ba-a10e-5e8a717a9fab" />
<br><br>
Sau khi hoàn thành người dùng có thể truy cập vào 2 trang web WordPress và Laravel bình thường

### SSL
Khi người dùng deply web lên cyber panel thì cyber panel sẽ hỗ trợ gán chứng chỉ SSL lên cho trang web. Trong trường hợp bị lỗi thì chỉ cần chọn `Issue SSL` khi đó panel sẽ tự sửa lỗi và gán lại cho người dùng
<br><br>
<img width="1920" height="1010" alt="Image" src="https://github.com/user-attachments/assets/79418be6-da4a-4732-9280-06eb737165af" />


## ProxyPass
### Tạo tài khoản admin
Để truy cập được OpenLiteSpeeed, người dùng cần tạo tài khoản và mật khẩu admin
```
/usr/local/lsws/admin/misc/admpass.sh
```
Người dùng sẽ điền tài khoản (mặc định là admin) và mật khẩu. Sau khi đã tạo người dùng sẽ truy cập vào `địa_chỉ_IP:7080` và đăng nhập vào.
<br><br>
<img width="642" height="262" alt="Image" src="https://github.com/user-attachments/assets/c7cb6b2b-e16c-4fa5-8e69-2472d5d1e758" />
<br><br>
### Tạo Proxy (External App)
Người dùng tạo Proxy trên trang của OpenLiteSpeed
<br><br>
<img width="1920" height="953" alt="Image" src="https://github.com/user-attachments/assets/09751910-8674-4897-96e7-2426bf556f77" />
<br><br>
<img width="1918" height="524" alt="Image" src="https://github.com/user-attachments/assets/22146857-9ad4-4614-b112-9a876e4048b0" />
<br><br>
<img width="1696" height="864" alt="Image" src="https://github.com/user-attachments/assets/63cddf03-0ad5-4aba-92ca-a644732bba59" />
<br><br>
### Cấu hình vhost vào proxy vừa tạo
Người dùng sẽ cấu hình trên vhost để domain trỏ vào proxy vừa tạo
```
context /api/ {
  type                    proxy
  handler                 testport
  addDefaultCharset       off
}
```
<br><br>
<img width="1918" height="996" alt="Image" src="https://github.com/user-attachments/assets/321c9e12-afb0-44dc-b4e7-a26f013b7c8b" />
<br><br>


### Tạo app chạy trên port 5000
Người dùng tiến hành tạo 1 app python đơn giản để chạy ở port 5000 và có endpoint là `api` để nhận request
```
  GNU nano 6.2                                                                                                        app.py                                                                                                                  
# Import thư viện Flask để tạo ứng dụng web.
from flask import Flask, jsonify

# Khởi tạo một đối tượng Flask.
app = Flask(__name__)
app.config['JSON_AS_ASCII'] = False

# Định nghĩa một route cho URL gốc '/'.
@app.route('/')
def home():
    return "Chào mừng bạn đến với ứng dụng Python đơn giản!"

# Định nghĩa một route cho URL '/api'.
# Đây là endpoint mà chúng ta sẽ sử dụng cho ProxyPass.
@app.route('/api/')
@app.route('/api')
def api_endpoint():
    # Trả về một phản hồi JSON.
    return jsonify({"message": "Đây là kết quả từ ứng dụng backend!"})

# Kiểm tra nếu script đang chạy trực tiếp.
if __name__ == '__main__':
    # Chạy ứng dụng trên cổng 5000.
    app.run(host='0.0.0.0', port=5000)
```
Sau đó chạy app này để client có thể truy cập và gửi request vào.
Cuối cùng nếu cấu hình đúng thì khi người dùng truy cập vào đường dẫn `wp.tule.vietnix.tech/api` thì sẽ có message trả về như hình
<br> <br>
<img width="827" height="204" alt="Image" src="https://github.com/user-attachments/assets/a7265b1a-e281-4a85-9612-a9de1e0b5bc5" />
<br><br>
<img width="1919" height="186" alt="Image" src="https://github.com/user-attachments/assets/8178f281-4254-4c0d-b65a-302e5c140471" />

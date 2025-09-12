# NỘI DUNG TÌM HIỂU 12/09/2025 
<br>


## CPANEL

### 1. Chuyển tài nguyên từ vps lên cpanel
Người dùng tiến hành upload các source code của cả 2 trang __WordPress__ và __Laravel__.
Trong đó do đường dẫn để WordPress lấy source trên Cpanel đã được cấu hình sẵn nên sau khi upload người dùng cần copy các file vào đúng đường dẫn đó:
```
scp -r ~/.ssh/vps_key /var/www/wp.tule.vietnix.tech wptulevi@host88.vpshosting.vn:/home/wptulevi/
```

  <img width="1919" height="300" alt="Image" src="https://github.com/user-attachments/assets/9fb58b47-90f7-4506-ab47-51b4265a1fd9" />  
  
Thực hiện tương tự với __Laravel__ nhưng về đường dẫn thì có thể tùy chỉnh trên __Cpanel__ sau.

### 2. Tạo domain cho `Laravel`
Do trên Cpanel thì domain `WordPress` đã được cấu hình từ trước nên người dùng chỉ cần thêm domain cho `Laravel`

<img width="1673" height="561" alt="Image" src="https://github.com/user-attachments/assets/77b21197-de5f-4680-ace5-039082822348" />  


<img width="1673" height="618" alt="Image" src="https://github.com/user-attachments/assets/aa4da8d5-5086-4f7b-8091-fcfdb0c0ce73" />  


<img width="896" height="763" alt="Image" src="https://github.com/user-attachments/assets/f8e46191-f155-4d5c-aedc-6b1bc5f746e7" />  


### 3. Tạo database
Có nhiều tùy chọn trong đó người dùng có thể chon __Database Wizard__ - tùy chọn này giúp tạo database và user với các quyền trên database đó

<img width="1669" height="1007" alt="Image" src="https://github.com/user-attachments/assets/070eb887-f796-41d7-893f-2b8f647ce3f2" />

<img width="908" height="373" alt="Image" src="https://github.com/user-attachments/assets/47b61cf3-5aef-4529-b197-f3b4987679aa" />

<img width="1045" height="624" alt="Image" src="https://github.com/user-attachments/assets/946a0a67-689e-4d08-9bb2-39551a3d8f28" />

<img width="1424" height="910" alt="Image" src="https://github.com/user-attachments/assets/a9eec59b-fd0d-4748-9f42-f70340c52e45" />

* Import data:

<img width="1673" height="858" alt="Image" src="https://github.com/user-attachments/assets/25fa1a7e-89a9-4bf9-9602-ea2f8f5b7d56" />

Người dùng tiến hành upload file sql ở vps lên cPanel
### 3. Config cho Laravel và WordPress truy cập được database
Người dùng sẽ thay đổi lại các thông tin của database trùng với database vừa được tạo trên cPanel

<img width="1674" height="374" alt="Image" src="https://github.com/user-attachments/assets/33d25128-01da-49fa-85cf-3f7f69b0c589" />

<img width="1674" height="669" alt="Image" src="https://github.com/user-attachments/assets/97267ba5-823b-49c1-8fa0-889083a00cba" />


* Lưu ý:  
Hiện tại 2 domain đang được cấu hình để trỏ về IP gốc của VPS, vì trong bài viết này việc sử dụng cPanel và không có quyền để trỏ DNS về địa chỉ của IP. Vì vậy để có thể truy cập vào domain và khiến domain đó truy cập vào đúng IP của cPanel __(Chỉ có tác dụng tại thiết bị người dùng)__  
Thêm các dòng này vào file `/etc/hosts`. Điều này sẽ giúp domain trỏ về cPanel.

<img width="1069" height="279" alt="Image" src="https://github.com/user-attachments/assets/15762b35-2619-4d9b-8f03-b340f16d6b37" />

### Kiểm tra
Dùng lệnh `curl` để kiểm tra xem trang web hoạt động bình thường. Ngoài ra, có thể dùng thêm lệnh `nslookup` để xem domain có trỏ về đúng địa chỉ không

<img width="1920" height="377" alt="Image" src="https://github.com/user-attachments/assets/f08c0ab4-06ff-403b-a41c-19c548957a62" />

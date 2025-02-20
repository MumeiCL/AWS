# Cài đặt webserver WordPress sử dụng máy chủ ảo EC2 và cơ sở dữ liệu RDS
## 1. Tạo RDS Database và EC2 instance
- Trong Create database page, thiết đặt các tham số sau:
- Chọn Standard create.
- Dưới Engine options, select MySQL.
- Chọn Version: MySQL 8.0.32.
- Dưới Templates, select Free tier
- Dưới DB instance identifier, đặt tên là mydatabase và copy nó vào clipboard.
- Sử dụng user quản trị DB là admin tại mục username .
- Dưới DB instance class, chọn db.t3.micro.
- Dưới Connectivity, Chọn the existing VPC và để Don't connect to an EC2 compute resource selected.
- Dưới VPC security group, chọn Create new VPC security group, đặt tên trong: New VPC security group name là: mysql-security.
- Dưới Availability zone chọn us-east-1a.
- Mở rộng Additional configuration and, dưới Initial database name, đặt tên là mydatabase.
- Dưới Backups, uncheck Enable automatic backups option
## 2. Cài đặt apache và Dependencies
- Cập nhật cho HĐH Ubuntu

 `sudo apt-get update -y`
- Cài đặt Apache webserver và Dependencies:

 `sudo apt install apache2 -y`
- Cài đặt các thư viện, PHP và PHP MySQL:

 `sudo apt -y install php php-bz2 php-mysqli php-curl php-gd php-intl php-common php-mbstring php-xml`
## 3. Cấu hình WordPress
- Chuyển hướng vào thư mục tạm /tmp:
  
`cd /tmp`
- download zip file wordpress về máy:
  
`curl -O https://wordpress.org/latest.tar.gz`
- Giải nén file:
  
`tar xzvf latest.tar.gz`
- Copy Sample config file WP để tạo file cấu hình cho wordpress:
  
`cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php`
- Tạo thư mục cập nhật cho wordpress:
  
`mkdir /tmp/wordpress/wp-content/upgrade`
- Copy thư mục wordpress vào thư mục www của apache web server:
  
`sudo cp -a /tmp/wordpress/. /var/www/wordpress`
- Cấu hình quyền truy cập cho người dùng từ internet có thể truy cập được vào wordpress:
```
sudo chown -R www-data:www-data /var/www/wordpress
sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \;
sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \;
```
- Tạo WordPress secret key :

`curl -s https://api.wordpress.org/secret-key/1.1/salt/`

- copy key, sau đó mở file cấu hình của wordpress:

`sudo nano /var/www/wordpress/wp-config.php`
- Tìm nội dung như trên, xóa toàn bộ đi và dán nội dung vừa copy vào.

- Tạo Apache configuration file và copy file này vào `/etc/apache2/sites-enabled/` để bật WordPress website để làm việc với /var/www/wordpress.
  
`sudo -i`
- Tạo file 000-default.conf:
  
`sudo nano 000-default.conf`
- Sau đó copy toàn bộ nội dung bên dưới và dán vào file 000-default.conf :
```
<VirtualHost *:80>
  ServerAdmin admin@test.com
  DocumentRoot /var/www/wordpress
  ServerName wordpress.test
        <Directory /var/www/wordpress>
            Options FollowSymlinks
            AllowOverride All
            Require all granted
        </Directory>
ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
-  Chuyển file 000-default.conf để tạo host ảo cho phép wordpress được truy cập từ internet qua Apache
  
`sudo mv 000-default.conf /etc/apache2/sites-enabled/`
- Restart the Apache 2 configuration:
  
`sudo apache2ctl restart`
- Trở lại trình duyệt web browser nơi có RDS Databases được mở.
- Trong Connectivity & security tab, dưới Endpoint, copy nội dung endpoint .
- Mở WordPress config PHP file cho soạn thảo:

`sudo nano /var/www/wordpress/wp-config.php`
- Thay đổi nội dung:
![image](https://github.com/user-attachments/assets/c7f07f19-65d7-4a6e-864e-10373297371d)

dán nội dung endpoint vào DB_HOST

## 4. Cấu hình Security Group
Thêm inbound rule cho phép kết nối tới máy ảo 
sg-0954782c017ead9c9 (launch-wizard-4) là security group của webserver01
![image](https://github.com/user-attachments/assets/f79bee35-cf52-454d-9d81-bf896ae1f6ca)

## 5. Đăng nhập và cấu hình cơ bản
Truy cập địa chỉ ip public máy ảo webserver01 để cấu hình wordpress
![image](https://github.com/user-attachments/assets/738fca64-418d-434f-89bc-91843a044306)

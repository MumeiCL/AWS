# Mô hình
![image](https://github.com/user-attachments/assets/392514cd-ba14-484c-8a91-75333343a8a8)

**Cài đặt các máy ảo webserver và các dịch vụ. Webserver01 sẽ cài thủ công, webserver02 sẽ cài tự động**

## Webserver01
Update và setup packages: `sudo apt-get update && sudo apt-get upgrade -y`

Cài đặt apache2 web server: `sudo apt-get install apache2 -y`

Cài đặt unzip: `sudo apt-get install unzip -y`

Download AWS CLI tool: `curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"`

Giải nén Unzip file: `unzip awscliv2.zip`

Setup AWS CLI tool: `sudo ./aws/install`

Xác nhận AWS CLI version 2 đã được cài đặt: `aws --version`
![image](https://github.com/user-attachments/assets/eaf04f86-e2bf-44ee-8961-1a08ff0ba373)

Cài đặt mysql: `sudo apt-get install mysql-server -y`

Cấp quyền cho file index.html: `sudo chmod 777 /var/www/html/index.html`

Lấy instances metadata và thêm vào file index.html
```
echo '<html><h1>Bootstrap Demo</h1><h3>Availability Zone: ' > /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/placement/availability-zone >> /var/www/html/index.html
echo '</h3> <h3>Instance Id: ' >> /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/instance-id >> /var/www/html/index.html
echo '</h3> <h3>Public IP: ' >> /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/public-ipv4 >> /var/www/html/index.html
echo '</h3> <h3>Local IP: ' >> /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/local-ipv4 >> /var/www/html/index.html
echo '</h3></html> ' >> /var/www/html/index.html
```
Nhập ip instance vào trình duyệt
![image](https://github.com/user-attachments/assets/0ca55354-145d-423a-af55-1237b5155d91)
## Webserver02
**Thêm user data lúc tạo instance**
```
#!/bin/bash
sudo apt-get update -y
sudo apt-get install apache2 unzip -y
sudo systemctl enable apache2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
echo '<html><h1>Bootstrap Demo</h1><h3>Availability Zone: ' > /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/placement/availability-zone >> /var/www/html/index.html
echo '</h3> <h3>Instance Id: ' >> /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/instance-id >> /var/www/html/index.html
echo '</h3> <h3>Public IP: ' >> /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/public-ipv4 >> /var/www/html/index.html
echo '</h3> <h3>Local IP: ' >> /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/local-ipv4 >> /var/www/html/index.html
echo '</h3></html> ' >> /var/www/html/index.html
sudo apt-get install mysql-server -y
sudo systemctl enable mysql
```
Nhập ip instance vào trình duyệt:

![image](https://github.com/user-attachments/assets/71609d55-d301-43e5-913b-fc14f8f888f0)

Kiểm tra xem apache và mysql đã được cài đặt chưa:

![image](https://github.com/user-attachments/assets/e7164604-2a78-4749-9bfd-70cadae8a377)

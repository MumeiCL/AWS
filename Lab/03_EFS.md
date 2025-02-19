# Cắt giảm chi phí lưu trữ dữ liệu trong AWS sử dụng EFS
**Mô hình**

![image](https://github.com/user-attachments/assets/1e33dd21-a0e5-4922-9f2b-9d20827c861c)

**Sửa đổi 03 máy chủ EC2 instances để sử dụng một dịch vụ lưu trữ chia sẻ file EFS thay vì dùng 03 ổ EBS volumes trùng lặp cho 03 máy chủ EC2 instances này(3 máy có dữ liệu như nhau). Việc này làm giảm đáng kể chi phí sử dụng lưu trữ dữ liệu.**

## Tạo 3 instances trùng lặp
![image](https://github.com/user-attachments/assets/3dc535e2-c860-40bb-a0a2-24f05ae026ad)

Điền 3 tại Summary để tạo 3 instances
```
#!/bin/bash
sudo yum update -y
sudo mkfs -t xfs /dev/xvdb
sudo -i
mkdir /data
mkdir /efs
echo '/dev/xvdb /data xfs defaults,nofail 0 2' >> /etc/fstab
sudo mount -a
cd /data/
sudo touch file01.txt file02.txt file03.txt file04.txt file05.txt file06.txt file07.txt file08.txt
file09.txt file10.txt
```

## Tạo EFS 
Tạo 1 EFS với storage class: one zone
![image](https://github.com/user-attachments/assets/2173345c-7308-4ca6-a72c-33ff9487a0a6)

## Chỉnh sửa security group của EFS
![image](https://github.com/user-attachments/assets/3af7adc3-9a6e-41c5-ac3c-65cbca475903)

Chỉnh sửa rule cho phép thực hiện kết nối ra ngoài
![image](https://github.com/user-attachments/assets/e9c83cd0-1af6-44af-a8fc-f53f3e49ad32)

## 
Tại tab EFS chọn attach sau đó chọn mount via IP để lấy câu lệnh mount
![image](https://github.com/user-attachments/assets/57c606d2-a9b3-4ad2-916c-867921d3b16c)

**Thực hiện chạy trên instance**

`sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 172.31.42.84:/ /efs`

**Chú ý thêm dấu / phía trước efs**

**Chuyển tất các các files from /data đến /efs file system:**
`sudo rsync -rav /data/* /efs`

![image](https://github.com/user-attachments/assets/5537f567-c191-445d-8355-590f699835f1)

**Loại bỏ dữ liệu cũ:**

`sudo umount /data`

Chỉnh sửa file /etc/fstab `sudo nano /etc/fstab`

![image](https://github.com/user-attachments/assets/d968325c-4b1d-4cb7-a949-16bf5af57365)

thay `/dev/xvdb /data xfs defaults,nofail 0 2` thành `172.31.42.84:/        /efs     nfs4      defaults,_netdev 0 0`

`sudo umount /efs`

![image](https://github.com/user-attachments/assets/446b9b77-fae3-4a0b-82d3-83b3c80649e9)

Từ giờ mỗi khi khởi instance sẽ tự động mount đến EFS đã cấu hình

**Dettach and delete volume**

![image](https://github.com/user-attachments/assets/383d3915-7daa-4d2b-8205-5b722d25f80b)

**Thực hiện tương tự với 2 máy còn lại**

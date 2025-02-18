**AWS CLI là công cụ dòng lệnh (command line) giúp quản lý và tương tác với các dịch vụ AWS trực tiếp từ terminal hoặc script. Với AWS CLI, bạn có thể tự động hóa việc triển khai và vận hành tài nguyên trên AWS một cách linh hoạt, thay vì phải thao tác thủ công trên AWS Management Console.**



## Tạo một EC2 Instances (sử dụng AWS CLI trong máy ảo này)

![image](https://github.com/user-attachments/assets/be48b739-2a33-4b62-85a6-308c17af5e03)

## Tạo IAM User có quyền truy cập và tạo tài nguyên trong S3

![image](https://github.com/user-attachments/assets/2b875387-4d95-47ef-8bee-ed9737520df3)

**Để CLI có thể tương tác với tài khoản AWS của bạn, cần cấu hình Access Key và Secret Access Key.
Vào AWS Management Console → IAM → Users → Chọn user → Security credentials → Create access key.**
![image](https://github.com/user-attachments/assets/dcec23fc-baa5-4de2-8c8a-c9775dca887a)


## Sử dụng IAM user's credentials để cấu hình và sử dụng AWS CLI
-**Cấu hình CLI:**
`aws configure`
Sau đó nhập:
```
AWS Access Key ID
AWS Secret Access Key
Default region name (ví dụ: us-east-1, ap-southeast-1)
Default output format (json, text, hoặc table)
```
![image](https://github.com/user-attachments/assets/62ecba40-1ce9-403e-8992-57bf832f8cd9)


![image](https://github.com/user-attachments/assets/4931f245-8006-46a7-a926-00df3d73e865)

## Các lệnh cơ bản
**Kiểm tra phiên bản**
`aws --version`

**Lệnh quản lý S3**
- Liệt kê tất cả bucket: `aws s3 ls`
- Liệt kê object trong một bucket: `aws s3 ls s3://my-bucket`
- Tải lên (upload) file: `aws s3 cp localfile.txt s3://my-bucket/`
- Tải xuống (download) file: `aws s3 cp s3://my-bucket/remote-file.txt .`
- Đồng bộ thư mục: `aws s3 sync ./local-folder s3://my-bucket/remote-folder`
- 
**Lệnh quản lý EC2**
- Liệt kê instance: `aws ec2 describe-instances`
- Khởi chạy EC2 (ví dụ đơn giản):
```
aws ec2 run-instances \
  --image-id ami-12345678 \
  --instance-type t2.micro \
  --count 1 \
  --subnet-id subnet-abc123 \
  --security-group-ids sg-xyz789 \
  --key-name MyKeyPair
```
- Dừng instance: `aws ec2 stop-instances --instance-ids i-0123456789abcdef0`
- Xóa instance: `aws ec2 terminate-instances --instance-ids i-0123456789abcdef0`
- 
**Lệnh quản lý IAM**
- Liệt kê user: `aws iam list-users`
- Tạo user: `aws iam create-user --user-name mynewuser`

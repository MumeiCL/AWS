**IAM roles cho phép cấp quyền xác thực tạm thời cho EC2 Instance để truy cập vào các tài nguyên và dịch vụ AWS. Sau đó, các thông tin đăng nhập tạm thời này có thể được sử dụng bởi các ứng dụng chạy trên EC2 để truy cập vào các tài nguyên AWS với các quyền được định rõ trong cấu hình của roles.
IAM Role giúp Loại bỏ nhu cầu quản lý thông tin đăng nhập (credentials) dài hạn, giúp giảm thiểu rủi ro bảo mật. Đơn giản hóa việc quản lý phân quyền.**
## Mục tiêu lab
Chỉ cấp quyền cho EC2 instance truy cập vào 2 bucket của bộ phận dev

## Thực hiện
**Tạo 1 instance**
![image](https://github.com/user-attachments/assets/3da40821-484f-4e5f-ae70-f599ee63e627)

**Tạo 4 bucket**
![image](https://github.com/user-attachments/assets/e0598809-e3c8-44cb-849f-f8e13b61fa3b)

**Tạo Policy**
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
         "Sid": "AllowUserToSeeBucketListInTheConsole",
         "Action": ["s3:ListAllMyBuckets", "s3:GetBucketLocation"],
         "Effect": "Allow",
         "Resource": ["arn:aws:s3:::*"]
        },
        {
         "Effect": "Allow",
         "Action": [
            "s3:Get*",
            "s3:List*"
         ],
         "Resource": [
           "arn:aws:s3:::<S3_BUCKET_NAME>/*",
           "arn:aws:s3:::<S3_BUCKET_NAME>"
         ]
        }
    ]
}
```

**Tạo role**
![image](https://github.com/user-attachments/assets/6cb5b9f4-0f5a-4813-b251-f48925d0c0c6)

**Gán role cho EC2 instance**
Click Actions > Security > Modify IAM role.

![image](https://github.com/user-attachments/assets/aa0a0417-2724-4b16-8661-eaeb5584b569)

**Kiểm tra**

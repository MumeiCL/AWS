#
## Quản lý chính sách, tạo và phân quyền users, groups
**Mô Hình:**

![image](https://github.com/user-attachments/assets/ef137433-9924-4ed0-8959-9d9fee21a569)

**B1: Tạo user**
![Untitled](https://github.com/user-attachments/assets/e00d6952-0d3a-4959-90fb-8d477d37d85e)

![image](https://github.com/user-attachments/assets/7d59492f-a0b3-4a73-bc35-1a2f258d7554)

![image](https://github.com/user-attachments/assets/fbc5e5e3-0eed-4c8f-9f10-5e5b3c57e266)

**Tải file .csv chưa thông tin tài khoản user vừa tạo**
![image](https://github.com/user-attachments/assets/a0f954fa-1bbb-4252-958c-f4bf72bcc3a1)

**Tương tự với 2 user còn lại**
![image](https://github.com/user-attachments/assets/02c2ab17-05b6-4d70-9cdd-465f033f0ace)

**Tạo user group**
![Untitled1](https://github.com/user-attachments/assets/3b9280c0-e770-4042-8b25-651a3c2e9f6c)

![image](https://github.com/user-attachments/assets/46ed565e-98ec-40ad-9097-b12f5369de93)

**Nội dung file JSON cấu hình quyền cho group EC2-Admin**
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ec2:Describe*",
                "ec2:StartInstances",
                "ec2:StopInstances"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": "elasticloadbalancing:Describe*",
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "cloudwatch:ListMetrics",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:Describe*"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": "autoscaling:Describe*",
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```

![image](https://github.com/user-attachments/assets/e9d356a1-7e8b-4f25-b541-37efdec056b7)
**Attach permision policy cho 2 user còn lại**
![image](https://github.com/user-attachments/assets/25187414-d7f1-4ca4-adfe-74ec986a2562)

![image](https://github.com/user-attachments/assets/7f5bbb62-77b3-44a7-842c-f609fd0c4064)

## Thực hành tạo và Assume Roles trong AWS
**Mô hình**

![image](https://github.com/user-attachments/assets/7e94cf54-47f5-4e08-8e71-e7ad6d7a0fd5)

**Nhân viên dev-user1 có quyền truy cập vào 2 bucket appconfig, dev-user2 tiến hành assume role của dev-user1 để có thể truy cập tạm thời vào 2 bucket appconfig**

**B1: tạo 2 user dev-user1 và dev-user2**
![image](https://github.com/user-attachments/assets/100dcc39-3bb7-4a77-9a15-1293f15352f0)

**B2: Vào dịch vụ S3 và tạo 4 bucket**
![image](https://github.com/user-attachments/assets/8d18a068-e948-487e-af85-eafb72a4f23f)

**B3: Tạo policy mới**
![Untitled2](https://github.com/user-attachments/assets/8842fe24-d97b-44ac-b09e-38053f190307)

**Chọn dịch vụ S3 và allow all s3 action**
![image](https://github.com/user-attachments/assets/240966c2-f841-40cf-a25e-c5ce6fcb3784)

**Cấu hình Resources chỉ cho phép truy cập 2 bucket appconfig**
![image](https://github.com/user-attachments/assets/cf86f15e-1f8e-4603-924b-64c72ec95eee)

**B4: attach dev-user1 vào policy**
![image](https://github.com/user-attachments/assets/f99b3345-1a65-45f2-bb17-badb539cdae4)

**B5: Tạo role mới**
![Untitled3](https://github.com/user-attachments/assets/fafb82c0-258e-40f8-9dfa-516ef3048756)

![image](https://github.com/user-attachments/assets/75129c30-e295-4509-862b-601f8d5be1ef)

![image](https://github.com/user-attachments/assets/034da5ad-f30f-4b6f-905a-4ccba5f92511)

![image](https://github.com/user-attachments/assets/d3e6f2eb-2e33-46a8-88b5-203f155d59f8)

**B6: Sửa trust policy**
![image](https://github.com/user-attachments/assets/a61ccfe2-5073-4416-9186-1b1ba15649a4)
**Sửa arn của tài khoản root sang tài khoản dev-user2**
![image](https://github.com/user-attachments/assets/ddd5f579-5df3-4e97-ad6f-b65d4581ecdd)

# END

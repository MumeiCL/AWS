# IAM Policy trong AWS
- IAM Policy (Chính sách IAM) là một tập hợp các quy tắc xác định quyền truy cập vào tài nguyên AWS. IAM Policy giúp kiểm soát chi tiết (fine-grained access control) cho người dùng, nhóm, vai trò (roles) hoặc dịch vụ trong AWS.
## Cấu trúc của IAM Policy
**IAM Policy được viết dưới dạng JSON với các thành phần chính:**

| **Thành phần**  | **Ý nghĩa**  | **Ví dụ**  |
|---------------|------------|------------|
| **Version**   | Phiên bản của policy  | `"Version": "2012-10-17"`  |
| **Statement** | Danh sách các điều kiện cấp quyền  | Chứa một hoặc nhiều **Action**, **Effect**, **Resource**, **Condition**  |
| **Effect**    | Quy định **cho phép (Allow)** hoặc **từ chối (Deny)** hành động  | `"Effect": "Allow"` hoặc `"Effect": "Deny"`  |
| **Action**    | Các hành động AWS được phép hoặc bị từ chối  | `"Action": "s3:ListBucket"`  |
| **Resource**  | Tài nguyên AWS áp dụng chính sách  | `"Resource": "arn:aws:s3:::my-bucket"`  |
| **Condition** *(Không bắt buộc)* | Điều kiện cụ thể để policy áp dụng  | Chỉ cho phép truy cập từ IP nhất định, MFA, thời gian trong ngày,...  |

## Các loại IAM Policy trong AWS  

| **Loại** | **Mô tả** | **Ví dụ** |
|---------|----------|-----------|
| **Managed Policy** | Chính sách do AWS hoặc người dùng tạo, có thể gán cho nhiều thực thể | `AdministratorAccess`, `ReadOnlyAccess` |
| **Inline Policy** | Chính sách gán trực tiếp vào một user, group hoặc role (không thể tái sử dụng) | Một policy chỉ cấp quyền `s3:PutObject` cho một user cụ thể |
| **Service Control Policy (SCP)** | Chính sách áp dụng cho toàn bộ tài khoản trong AWS Organizations | Chặn xóa S3 bucket trên toàn bộ tài khoản |
| **Permission Boundary** | Giới hạn quyền tối đa mà một user/role có thể có | Hạn chế user chỉ có thể thao tác với EC2, không được truy cập S3 |

## Best Practices khi sử dụng IAM Policy
- Dùng nguyên tắc "Least Privilege" (Quyền tối thiểu): Chỉ cấp quyền cần thiết nhất.
- Dùng IAM Role thay vì IAM User khi cấp quyền cho dịch vụ hoặc ứng dụng.
- Sử dụng AWS Managed Policies khi có thể để giảm rủi ro sai sót.
- Kích hoạt MFA để tăng cường bảo mật.
- Giám sát quyền truy cập với AWS IAM Access Analyzer.
- Hạn chế quyền AdministratorAccess, chỉ cấp cho user thực sự cần thiết.

## Ví dụ về IAM Policy
**Policy chỉ cho phép đọc dữ liệu từ S3**
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::my-bucket",
        "arn:aws:s3:::my-bucket/*"
      ]
    }
  ]
}
```

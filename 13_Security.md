# Security
## CloudTrail
**AWS CloudTrail là một dịch vụ ghi lại tất cả hoạt động của người dùng, API, và các sự kiện trong AWS. Dữ liệu này giúp giám sát, phát hiện lỗi, và kiểm tra bảo mật.**
### Chức năng chính
- Ghi lại sự kiện (Event Logging): Ghi lại các API calls, bao gồm người dùng, thời gian, nguồn IP, và các thay đổi.
- Phân tích & Giám sát bảo mật: Hỗ trợ phát hiện các hành vi bất thường hoặc truy cập trái phép.
- Điều tra & Kiểm tra tuân thủ: Giúp doanh nghiệp kiểm tra tính toàn vẹn và tuân thủ quy định.
- Tích hợp với AWS Services: CloudTrail có thể kết hợp với Amazon CloudWatch Logs, AWS Lambda, và SIEM để tự động phản hồi sự kiện.
### Các loại sự kiện trong CloudTrail
- Management Events: Các thao tác quản lý tài nguyên (tạo, xóa, sửa đổi).
- Data Events: Hoạt động trên dữ liệu (truy cập S3, DynamoDB, Lambda...).
- Insight Events: Phát hiện các hoạt động bất thường trong tài khoản AWS.
### Cách hoạt động
- Khi một API request được thực hiện trên AWS, CloudTrail sẽ ghi lại thông tin như:
  - Ai đã thực hiện hành động?
  - Hành động gì đã xảy ra?
  - Khi nào và từ đâu hành động được thực hiện?
- CloudTrail có thể lưu trữ logs trong S3, gửi logs đến CloudWatch, hoặc xuất ra SNS để thông báo.
## WAF & Shield
**AWS cung cấp AWS WAF và AWS Shield để bảo vệ các ứng dụng web khỏi các mối đe dọa như DDoS, SQL Injection, XSS, và các tấn công khác.**
### WAF (Web Application Firewall)
- Bảo vệ khỏi các cuộc tấn công phổ biến như SQL Injection, Cross-Site Scripting (XSS), Bot Traffic.
- Tạo & quản lý quy tắc lọc request theo IP, headers, URI, body, hoặc tốc độ request.
- Giám sát & Logging: Kết hợp với AWS CloudWatch để theo dõi lưu lượng.
- Tích hợp với các dịch vụ AWS khác như CloudFront, ALB, API Gateway, App Runner.
- AWS WAF hoạt động theo mô hình Rule-Based Filtering (lọc theo quy tắc), bao gồm:
  - Web ACL (Access Control List): Tập hợp các quy tắc để chặn hoặc cho phép request.
  - Rules: Các điều kiện để kiểm tra request.
  - Rule Groups: Tập hợp nhiều rules để quản lý dễ dàng.
### Shield (Bảo vệ chống DDoS)
- AWS Shield Standard (Miễn phí)
  - Bảo vệ chống DDoS Layer 3/4 (SYN Floods, UDP Reflection, etc.).
  - Tự động phát hiện & giảm thiểu DDoS.
  - Áp dụng mặc định cho tất cả dịch vụ AWS (CloudFront, ALB, Route 53, etc.).
- AWS Shield Advanced (Trả phí)
  - Bảo vệ nâng cao chống DDoS Layer 3, 4 & 7.
  - Tích hợp với AWS WAF để ngăn chặn Layer 7 (Application Layer).
  - Phân tích thời gian thực, hỗ trợ chuyên gia AWS 24/7.
  - Bồi hoàn chi phí AWS nếu có thiệt hại do DDoS.
  - Tích hợp AWS Firewall Manager để quản lý tập trung.
## Macie
**AWS Macie là dịch vụ bảo mật sử dụng Machine Learning (ML) để phát hiện, phân loại và bảo vệ dữ liệu nhạy cảm trong Amazon S3. Nó giúp tự động nhận diện thông tin như dữ liệu cá nhân (PII), thông tin thẻ tín dụng, thông tin y tế và cảnh báo về rủi ro bảo mật dữ liệu.**
### Chức năng chính
- Phát hiện dữ liệu nhạy cảm: Tự động quét các bucket S3 để tìm dữ liệu như số thẻ tín dụng, số CMND, địa chỉ email...
- Phân loại & Đánh giá rủi ro dữ liệu: Xác định dữ liệu nào là nhạy cảm và đánh giá mức độ rủi ro.
- Giám sát & Cảnh báo bảo mật: Tạo cảnh báo khi phát hiện dữ liệu bị công khai, quyền truy cập không an toàn, hoặc có hành vi bất thường.
- Tích hợp với các dịch vụ AWS khác: AWS Security Hub, AWS CloudWatch, AWS Lambda để tự động phản hồi sự cố.
### Cách hoạt động
- Bước 1: Phát hiện dữ liệu nhạy cảm
  - Macie quét các Amazon S3 Buckets theo lịch trình hoặc yêu cầu thủ công.
  - Dữ liệu được so sánh với mô hình ML & danh mục dữ liệu nhạy cảm có sẵn.
- Bước 2: Đánh giá mức độ rủi ro
  - Macie kiểm tra quyền truy cập bucket S3, mức độ công khai, quyền chia sẻ.
  - Xác định mức độ phơi nhiễm dữ liệu, như dữ liệu bị công khai trên internet.
- Bước 3: Tạo cảnh báo & đề xuất khắc phục
  - Khi phát hiện vi phạm bảo mật dữ liệu, Macie sẽ gửi cảnh báo đến AWS Security Hub, CloudWatch, SNS.
  - Có thể kết hợp AWS Lambda để tự động thực hiện hành động (VD: Chặn quyền truy cập bucket).
## Presigned URL
**Presigned URL là đường dẫn tạm thời do AWS S3 tạo ra, cho phép truy cập vào một tệp (object) cụ thể trong S3 mà không cần cấp quyền trực tiếp.**
### Cách hoạt động
- Người dùng/AWS service tạo một Presigned URL thông qua AWS SDK hoặc AWS CLI.
- Presigned URL chứa chữ ký số xác thực request và thời gian hết hạn.
- Người nhận có thể sử dụng URL để tải lên hoặc tải xuống object trong khoảng thời gian cho phép.
### Trường hợp sử dụng Presigned URL
- Tải file từ S3 (GET): Cho phép người dùng tải file mà không cần cấp quyền trực tiếp trên S3.
- Tải file lên S3 (PUT/POST): Ứng dụng client có thể tải file lên mà không cần quyền IAM.
- Chia sẻ file tạm thời: Chỉ cho phép truy cập file trong thời gian ngắn (VD: 10 phút).
## Inspector
**AWS Inspector là dịch vụ quét bảo mật tự động giúp phát hiện lỗ hổng bảo mật và kiểm tra tuân thủ trên hệ thống AWS**
### Các tính năng chính
- Quét lỗ hổng bảo mật (Vulnerability Scanning)
  - Tự động phát hiện lỗ hổng bảo mật trên EC2, container, Lambda
  - Cập nhật liên tục dữ liệu về CVE (Common Vulnerabilities and Exposures)
  - Chấm điểm rủi ro theo tiêu chuẩn CVSS (Common Vulnerability Scoring System)
- Kiểm tra tuân thủ bảo mật (Compliance Checks)
  - Kiểm tra tuân thủ CIS Benchmarks, NIST, PCI DSS
  - Đề xuất cách khắc phục các cấu hình sai
- Quét container trong Amazon Elastic Container Registry (ECR)
  - Tự động quét hình ảnh container khi push lên ECR
  - Xác định lỗ hổng bảo mật trong dependencies của container
- Tích hợp với các dịch vụ AWS khác
  - AWS Security Hub → Quản lý và tổng hợp kết quả quét bảo mật
  - Amazon EventBridge → Kích hoạt cảnh báo khi phát hiện lỗ hổng
  - AWS Lambda → Xử lý tự động khi phát hiện vấn đề
## Cognito
**Amazon Cognito là dịch vụ xác thực và ủy quyền người dùng, giúp quản lý danh tính cho các ứng dụng web và mobile.**
- Hỗ trợ đăng nhập với email, số điện thoại, username
- Tích hợp với Google, Facebook, Apple, SAML, OIDC
- Quản lý và xác thực người dùng với IAM, API Gateway, Lambda

### Thành phần chính 
- User Pools (Kho người dùng)
  - Xác thực người dùng (Sign-up & Sign-in).
  - Lưu trữ thông tin người dùng (Tên, email, số điện thoại).
  - Tích hợp xác thực đa yếu tố (MFA).
  - Hỗ trợ liên kết đăng nhập từ Google, Facebook, Apple, OIDC, SAML.
- Ví dụ luồng đăng nhập với Cognito User Pool:
  - Người dùng nhập email & mật khẩu.
  - Cognito xác thực thông tin.
  - Nếu thành công, Cognito trả về ID Token (JWT).
  - Ứng dụng sử dụng ID Token để gọi API.

- Identity Pools (Liên kết danh tính với AWS IAM)
  - Cấp quyền truy cập AWS Services (S3, DynamoDB, API Gateway...) cho người dùng.
  - Xác thực từ User Pools, Google, Facebook, SAML, OIDC.
  - Sử dụng IAM Role để kiểm soát quyền truy cập AWS Resources.
- Ví dụ luồng cấp quyền với Identity Pool:
  - Người dùng đăng nhập qua Cognito User Pool.
  - Cognito chuyển đổi danh tính thành IAM Role.
  - Người dùng có thể truy cập S3, DynamoDB, Lambda dựa trên IAM Role.


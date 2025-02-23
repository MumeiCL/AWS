# Serverless 
## Lambda
**AWS Lambda là dịch vụ serverless của AWS giúp chạy code mà không cần quản lý server. Lambda tự động mở rộng, tính tiền theo số lần thực thi, và có thể tích hợp với nhiều dịch vụ AWS như API Gateway, S3, DynamoDB, SQS, SNS.**
### Đặc điểm chính
- Không cần quản lý server: Chạy code mà không cần quan tâm đến hạ tầng.
- Tự động mở rộng: Lambda tự động scale theo số lượng request.
- Chỉ trả phí khi sử dụng: Tính phí theo số lần gọi và thời gian chạy.
- Hỗ trợ nhiều ngôn ngữ: Python, Node.js, Java, Go, .NET, Ruby.
- Tích hợp với nhiều dịch vụ AWS: API Gateway, S3, DynamoDB, CloudWatch.
### Mô hình hoạt động
- Sự kiện kích hoạt (Trigger) từ API Gateway, S3, DynamoDB, CloudWatch, SNS, SQS.
- AWS Lambda thực thi code với input từ sự kiện.
- Lambda trả về kết quả cho dịch vụ gọi nó.
- CloudWatch ghi log về quá trình thực thi.
### Các thành phần chính
- Function (Hàm Lambda)
  - Là code chạy khi Lambda được kích hoạt.
  - Có thể viết bằng Python, Node.js, Java, Go, .NET, Ruby.
  - Mỗi function có timeout tối đa 15 phút.
- Trigger (Sự kiện kích hoạt)
  - Là dịch vụ hoặc sự kiện khởi chạy Lambda, ví dụ:
  - API Gateway → Lambda để xử lý HTTP request.
  - S3 → Lambda để xử lý file upload.
  - DynamoDB Streams → Lambda để xử lý thay đổi dữ liệu.
  - SQS, SNS, CloudWatch → Lambda để xử lý tin nhắn/sự kiện.
- Permissions (Quyền truy cập IAM)
  - Lambda cần IAM Role để truy cập các dịch vụ AWS khác.
  - Ví dụ: Nếu Lambda muốn đọc từ S3 và ghi vào DynamoDB, cần cấp quyền tương ứng.
- Execution Environment (Môi trường thực thi)
  - Mỗi Lambda chạy trong môi trường cô lập (sandbox).
  - Có thể dùng Layer để thêm thư viện ngoài.
### Quản lý Lambda
- Memory & Timeout
  - RAM tối đa: 10GB.
  - Timeout tối đa: 15 phút.
- Concurrency (Đồng thời)
  - Lambda có thể chạy nhiều instance song song.
  - Có thể giới hạn số lượng instance đang chạy.
- VPC Integration
  - Lambda có thể chạy trong VPC riêng để truy cập RDS, ElastiCache.
- Logging & Monitoring
  - Dùng AWS CloudWatch để ghi log và giám sát Lambda.
## AWS ECS & AWS EKS 
- AWS cung cấp hai dịch vụ chính để chạy container trên hạ tầng đám mây:
  - Amazon ECS (Elastic Container Service) – Dịch vụ chạy container dựa trên AWS Fargate hoặc EC2.
  - Amazon EKS (Elastic Kubernetes Service) – Dịch vụ Kubernetes được quản lý trên AWS.

# Decoupling Workflows
## Amazon SQS (Simple Queue Service)
**Amazon SQS là dịch vụ hàng đợi tin nhắn được quản lý hoàn toàn bởi AWS, giúp các thành phần của ứng dụng giao tiếp không đồng bộ và giảm tải cho nhau. Nó hỗ trợ độ trễ thấp, khả năng mở rộng cao, và đảm bảo tin nhắn không bị mất ngay cả khi một thành phần gặp sự cố.**
### Đặc điểm
- Decoupling (Tách rời thành phần): Giúp các ứng dụng xử lý tin nhắn mà không cần giao tiếp trực tiếp.
- Không cần quản lý server: AWS đảm nhận việc vận hành, mở rộng và bảo mật hàng đợi.
- Chịu lỗi cao: Tin nhắn được lưu trữ tạm thời và có thể phục hồi nếu bị lỗi.
- Bảo mật: Hỗ trợ mã hóa tin nhắn, kiểm soát truy cập bằng IAM, và tích hợp AWS KMS.
- Mở rộng dễ dàng: Hỗ trợ hàng triệu tin nhắn mỗi giây mà không bị nghẽn.
### Các loại hàng đợi
- Standard Queue
  - Độ trễ thấp (ít hơn 10ms).
  - Không đảm bảo thứ tự tin nhắn.
  - Có thể có tin nhắn trùng lặp.
=>  Sử dụng khi tốc độ quan trọng hơn tính nhất quán (ví dụ: xử lý log, gửi email).
- FIFO Queue (First-In-First-Out)
  - Đảm bảo đúng thứ tự tin nhắn.
  - Không có trùng lặp tin nhắn.
  - Tốc độ giới hạn (300 TPS mặc định, có thể mở rộng lên 3,000 TPS với batching).
=>  Sử dụng khi cần đúng thứ tự, không mất tin nhắn (ví dụ: giao dịch tài chính, đặt hàng).
### Một số cấu hình 
- Visibility Timeout
  - Khoảng thời gian ẩn tin nhắn sau khi một worker nhận tin nhắn để xử lý.
  - Nếu worker không xác nhận hoàn thành trong thời gian này, tin nhắn sẽ trở lại hàng đợi.
  - Mặc định: 30 giây (tối đa 12 giờ).
- Delay Queue (Hàng đợi trì hoãn)
  - Định nghĩa khoảng thời gian tin nhắn chưa thể được nhận sau khi gửi vào hàng đợi.
  - Dùng để trì hoãn xử lý tin nhắn (ví dụ: gửi thông báo nhắc nhở sau 5 phút).
  - Mặc định: 0 giây (tối đa 15 phút).
- Dead Letter Queue (DLQ - Hàng đợi tin nhắn lỗi)
  - Nếu một tin nhắn không được xử lý thành công sau một số lần thử (Maximum Receives), nó sẽ chuyển sang DLQ.
  - Dùng để gỡ lỗi và theo dõi tin nhắn bị lỗi.
## Amazon SNS (Simple Notification Service) 
**Amazon SNS là dịch vụ pub/sub messaging giúp gửi thông báo theo thời gian thực đến nhiều hệ thống hoặc người dùng cùng lúc. SNS hỗ trợ nhiều kênh nhận tin nhắn như SQS, Lambda, email, SMS, HTTP(S) giúp xây dựng hệ thống event-driven, giảm tải và tăng khả năng mở rộng.**
### Đặc điểm
- Mô hình Publish/Subscribe (Pub/Sub): Một Publisher (gửi thông báo) có thể gửi tin nhắn đến nhiều Subscriber (người nhận).
- Hỗ trợ nhiều loại Subscriber: Có thể gửi tin nhắn đến SQS, Lambda, email, SMS, HTTP(S) endpoints, Amazon Kinesis.
- Real-time event-driven: Tích hợp với các dịch vụ AWS khác để xử lý sự kiện gần như ngay lập tức.
- Không mất tin nhắn: Hỗ trợ cơ chế Message Delivery Retry nếu tin nhắn không được nhận.
- Bảo mật cao: Hỗ trợ mã hóa bằng AWS KMS, xác thực IAM, lọc quyền truy cập theo endpoint.
- Mở rộng không giới hạn: Có thể gửi hàng triệu tin nhắn mỗi giây mà không bị giới hạn hiệu suất.
### Mô hình hoạt động
- Publisher (Nhà phát hành) gửi tin nhắn đến Topic (chủ đề) trong SNS.
- SNS Topic phân phối tin nhắn đến các Subscriber đã đăng ký.
- Các Subscriber có thể là Lambda, SQS, email, SMS, HTTP(S), Kinesis.
- Nếu tin nhắn không được nhận, SNS có thể retry theo chính sách cấu hình.
### Các thành phần chính
- Topic (Chủ đề)
  - Là điểm trung gian để tập trung tin nhắn từ nhiều nguồn và phân phối đến nhiều Subscriber.
  - Mỗi topic có thể có nhiều subscriber.
- Subscriber (Người nhận)
  - Các hệ thống hoặc người dùng đăng ký nhận tin nhắn từ SNS Topic.
  - Các loại Subscriber phổ biến:
  - Amazon SQS: Hàng đợi tin nhắn để xử lý không đồng bộ.
  - AWS Lambda: Xử lý sự kiện ngay lập tức.
  - Email/SMS: Gửi thông báo trực tiếp đến người dùng.
  - HTTP(S) Endpoints: Webhook để tích hợp với ứng dụng bên ngoài.
  - Amazon Kinesis: Streaming dữ liệu theo thời gian thực.
- Message Filtering (Lọc tin nhắn)
  - Cho phép subscriber chỉ nhận những tin nhắn phù hợp với filter policy thay vì nhận tất cả tin nhắn.
  - Giúp giảm tải và tối ưu hiệu suất.
### Một số cấu hình 
- Message Delivery Policy (Chính sách gửi lại)
  - Nếu một subscriber không nhận tin nhắn, SNS có thể retry theo chính sách sau:
  - Retry Exponential Backoff: Gửi lại sau mỗi khoảng thời gian dài dần.
  - Retry tối đa X lần, sau đó có thể chuyển sang Dead Letter Queue (DLQ).
- Message Encryption (Mã hóa tin nhắn)
  - Hỗ trợ AWS KMS để mã hóa tin nhắn trong SNS.
- Access Policy (Chính sách truy cập)
  - Kiểm soát ai có quyền gửi tin nhắn hoặc ai có thể đăng ký nhận tin nhắn.
  - Dùng IAM Policy để giới hạn quyền truy cập.
## Amazon API Gateway
**Amazon API Gateway là dịch vụ quản lý API hoàn toàn serverless, giúp tạo, bảo mật, giám sát và mở rộng API cho ứng dụng web, mobile hoặc backend trên AWS. API Gateway hỗ trợ nhiều loại API như RESTful API, WebSocket API, HTTP API, và tích hợp dễ dàng với các dịch vụ khác như Lambda, DynamoDB, S3, SNS, SQS.**
### Đặc điểm chính
- Quản lý API hiệu quả: Giúp định tuyến, xác thực, theo dõi và bảo mật API.
- Tích hợp với AWS Lambda & Backend Services: API Gateway có thể gọi Lambda, EC2, DynamoDB, S3, SNS, SQS.
- Bảo mật API: Hỗ trợ xác thực bằng IAM, Lambda Authorizer, Cognito, API Key.
- Giới hạn tốc độ (Rate Limiting): Giúp bảo vệ API khỏi DDoS hoặc quá tải.
- Hỗ trợ WebSocket API: Giúp xây dựng ứng dụng real-time như chat, game.
- Hỗ trợ Caching: Giúp giảm tải backend bằng cách cache dữ liệu phản hồi.
### Các loại API
- REST API
  - API tiêu chuẩn, có nhiều tính năng nâng cao.
  - Hỗ trợ xác thực, logging, rate limiting, tích hợp dễ dàng với AWS.	
- HTTP API
  - Nhẹ hơn, nhanh hơn REST API.
  - Hỗ trợ JWT Authentication, tích hợp Lambda, HTTP backend.	
- WebSocket API
  - Hỗ trợ giao tiếp hai chiều real-time.
  - Giữ kết nối liên tục với client.
### Mô hình hoạt động
- Client (Người dùng/Ứng dụng) gửi request đến API Gateway.
- API Gateway xử lý request: Kiểm tra xác thực, kiểm soát truy cập, logging.
- API Gateway gọi backend: AWS Lambda, EC2, DynamoDB, S3, HTTP endpoints.
- Backend xử lý và trả kết quả về API Gateway.
- API Gateway phản hồi kết quả cho Client.
### Các tính năng quan trọng 
- Authentication & Authorization (Xác thực và Ủy quyền)
  - IAM Authentication: Chỉ cho phép user có quyền trong IAM truy cập API.
  - Lambda Authorizer: Dùng AWS Lambda để kiểm tra token (JWT, OAuth).
  - Amazon Cognito: Xác thực user qua Cognito User Pool.
  - API Key: Giới hạn truy cập API bằng key.
- Rate Limiting & Throttling (Giới hạn tốc độ)
  - Chặn request nếu vượt quá số lượng tối đa trong 1 giây, giúp bảo vệ API khỏi DDoS.
- Caching (Bộ nhớ đệm)
  - API Gateway hỗ trợ caching responses giúp giảm tải backend.
  - Có thể bật TTL Cache (1 giây – 3600 giây).
- Logging & Monitoring
  - Kết hợp với AWS CloudWatch để ghi log request/response.
  - Theo dõi hiệu suất API (latency, error rate).
- CORS (Cross-Origin Resource Sharing)
  - Cho phép API được truy cập từ các domain khác nhau (quan trọng với web apps).

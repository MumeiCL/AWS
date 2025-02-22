# AWS Elastic Load Balancing
## Tổng quan
**AWS Elastic Load Balancing (ELB) là một dịch vụ giúp tự động phân phối lưu lượng ứng dụng đến nhiều tài nguyên, như EC2 instances, containers (ECS, EKS), Lambda functions ở một hoặc nhiều Availability Zones (AZs).
ELB giúp tăng tính sẵn sàng (availability), khả năng mở rộng (scalability) và độ tin cậy (fault tolerance) của ứng dụng bằng cách đảm bảo lưu lượng được phân phối hiệu quả giữa các máy chủ backend.**
## Phân loại
|Loại ELB|	Mô tả	|Ứng dụng phổ biến|
|-----------|-----------------|--------------------|
|Application Load Balancer (ALB)|	Load Balancer hoạt động ở Lớp 7 (Application Layer), hỗ trợ HTTP, HTTPS, WebSocket, SSL/TLS	|Web applications, API Gateway, Microservices|
|Network Load Balancer (NLB)|	Load Balancer hoạt động ở Lớp 4 (Transport Layer), hỗ trợ TCP, UDP, TLS, có độ trễ thấp|	Real-time apps, gaming, VoIP, Financial trading|
|Gateway Load Balancer (GWLB)|	Load Balancer hoạt động ở Lớp 3 (Network Layer), giúp định tuyến lưu lượng đến các thiết bị bảo mật như Firewall, IDS/IPS	|Security Appliances, Intrusion Detection, Packet Filtering|

## Chi Tiết Của Từng Loại ELB
### Application Load Balancer (ALB)
- Hoạt động ở Lớp 7 (HTTP/HTTPS), hỗ trợ path-based và host-based routing.
- Có thể định tuyến dựa trên header, cookie, query string, source IP, method.
- Hỗ trợ WebSocket và tích hợp với AWS WAF để tăng cường bảo mật.
- Hỗ trợ gRPC và HTTP/2.
- Target Groups có thể là EC2, Lambda, Containers (ECS/EKS).
- Khi nào dùng ALB:
  - Khi cần cân bằng tải Web Applications, API Gateway, Microservices.
  - Khi cần path-based routing (ví dụ: /api đi đến nhóm backend API, /images đi đến nhóm backend images).
  - Khi muốn tích hợp với AWS WAF để bảo vệ chống lại các cuộc tấn công web.
### Network Load Balancer (NLB)
- Hoạt động ở Lớp 4 (TCP, UDP, TLS), xử lý hàng triệu requests mỗi giây.
- Độ trễ rất thấp (~milliseconds) vì không xử lý HTTP header.
- IP-based routing thay vì host/path-based như ALB.
- Có thể gán một static IP hoặc Elastic IP.
- Hỗ trợ TLS Termination và Proxy Protocol v2.
- Target Groups có thể là EC2, IP addresses, Containers.
- Khi nào dùng NLB:
  - Khi cần hiệu suất cao, độ trễ thấp (ví dụ: game servers, chat apps, financial trading).
  - Khi ứng dụng yêu cầu TCP, UDP, TLS, không phải HTTP/HTTPS.
  - Khi muốn gán static IP để client luôn kết nối với cùng một địa chỉ IP.
### Gateway Load Balancer (GWLB)
- Hoạt động ở Lớp 3 (Network Layer), dùng cho các thiết bị bảo mật như Firewall, IDS, IPS.
- Cho phép chuyển tiếp lưu lượng đến các thiết bị kiểm tra bảo mật trước khi đến đích.
- Sử dụng GENEVE Protocol (L3 encapsulation).
- Kết hợp với AWS Transit Gateway để quản lý luồng lưu lượng lớn.
- Khi nào dùng GWLB:
  - Khi muốn triển khai Firewall, IDS/IPS, Deep Packet Inspection (DPI).
  - Khi muốn chèn các thiết bị bảo mật bên thứ ba vào luồng lưu lượng AWS.
## Tính Năng Chung Của ELB
- Auto Scaling: Tự động điều chỉnh dựa trên tải của hệ thống.
- Multi-AZ Failover: Cân bằng tải giữa nhiều Availability Zones để tăng tính sẵn sàng.
- Sticky Sessions: ALB và NLB có thể duy trì kết nối của một user đến một backend cố định.
- Health Checks: Kiểm tra trạng thái của backend, nếu backend không khả dụng, ELB sẽ chuyển hướng traffic.
- SSL/TLS Termination: ALB và NLB hỗ trợ mã hóa TLS để bảo mật lưu lượng.
- Logging & Monitoring: Tích hợp với CloudWatch, AWS X-Ray, AWS WAF.

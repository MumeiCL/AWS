# Các nguyên tắc cơ bản về AWS 
## 1. Cơ sở hạ tầng toàn cầu AWS
### Khu vực (AWS Regions)
- AWS hiện có tổng số hơn 30 Regions trên toàn thế giới, mỗi Region là một tập hợp các trung tâm dữ liệu độc lập về mặt vật lý và cách ly logic để đảm bảo tính bảo mật và khả năng chịu lỗi.
- Mỗi Region được đặt tên theo khu vực địa lý, ví dụ: `us-east-1` (Bắc Virginia, Hoa Kỳ), `eu-west-1` (Ireland), `ap-southeast-1` (Singapore).
- Các Regions là một tập hợp các Availability Zones, các Regions phổ biến nhất thường có nhiều Availability Zones (tối thiểu là 3 AZs).
- Một số dịch vụ AWS chỉ có ở một số Regions cụ thể (ví dụ: AWS GovCloud chỉ dành cho cơ quan chính phủ Mỹ).
- AWS không tự động sao chép dữ liệu giữa các Regions (để tăng tính bảo mật và tuân thủ quy định).
- Hầu hết các tài nguyên được ràng buộc với một Regions cụ thể.
- Nên cài đặt các tài nguyên trong các AWS Regions gần với quốc gia.
### Vùng sẵn sàng (Availability Zones - AZs)
- Mỗi AZ là một hoặc nhiều trung tâm dữ liệu riêng biệt với nguồn điện, làm mát và kết nối mạng độc lập.
- Các AZs trong một Region được kết nối bằng mạng cáp quang tốc độ cao với độ trễ thấp (<2ms).
- Nếu một AZ gặp sự cố, hệ thống có thể tự động chuyển đổi sang AZ khác mà không ảnh hưởng đến dịch vụ.
- Một AZ có nhiều máy chủ(Server) mà bạn thuê sử dụng, dùng để triển khai các ứng dụng của bạn.
### Điểm hiện diện (Points of Presence - PoPs)
**PoPs là các điểm mở rộng hạ tầng AWS, giúp tăng tốc độ truy cập và tối ưu hóa hiệu suất. Có hai loại PoPs:**
- Edge Locations:
  - Một Edge Locations được sử dụng để làm bộ nhớ đệm (cache) chứa nội dung dữ liệu, giúp tăng tốc độ phân phối trong truy cập thông tin.
  - Các Edge Locations phục vụ dịch vụ Amazon CloudFront (CDN) để giảm độ trễ khi phân phối nội dung.
  - Khi người dùng truy cập nội dung từ một trang web sử dụng CloudFront, dữ liệu sẽ được lấy từ Edge Location gần nhất thay vì từ server gốc.
  - Giảm tốc độ xử lý trong kết nối mạng internet (Reduced latency).
- Regional Edge Caches:
  - Cải thiện hiệu suất cho nội dung tĩnh và động, lưu trữ các phiên bản dữ liệu phổ biến để giảm truy cập vào máy chủ gốc.
  - Hữu ích cho các ứng dụng yêu cầu tốc độ cao và độ trễ thấp.
### AWS Local Zones
- Local Zones là một phần mở rộng của AWS Region nhưng đặt gần hơn với người dùng cuối, giúp giảm độ trễ cho các ứng dụng thời gian thực như game, AR/VR, phát trực tiếp (livestream), và trí tuệ nhân tạo (AI/ML).
- Local Zones cung cấp các dịch vụ như EC2, EBS, VPC, FSx, Load Balancer, và RDS.
- Ví dụ: Một công ty game có thể sử dụng Local Zones ở Los Angeles để giảm độ trễ cho người chơi trong khu vực
### AWS Outposts
**AWS Outposts cho phép triển khai cơ sở hạ tầng AWS ngay tại trung tâm dữ liệu của khách hàng.**
- Hỗ trợ các dịch vụ như EC2, EBS, RDS, ECS, EKS, VPC nhưng chạy ngay tại chỗ thay vì trên AWS Cloud.
- Hữu ích cho các công ty yêu cầu độ trễ thấp, tuân thủ quy định chặt chẽ hoặc cần tích hợp với hệ thống hiện có.ư
- AWS chịu trách nhiệm bảo trì phần cứng và cập nhật phần mềm.
- Ví dụ: Ngành tài chính, chăm sóc sức khỏe, quân đội thường sử dụng AWS Outposts để đảm bảo dữ liệu không rời khỏi cơ sở.
### Mạng lưới kết nối toàn cầu của AWS
**AWS đầu tư mạnh vào hạ tầng mạng toàn cầu, bao gồm:**
- AWS Global Backbone
  - AWS sử dụng mạng cáp quang chuyên dụng có độ trễ thấp, tốc độ cao giữa các Regions.
  - Mạng AWS được bảo vệ bởi AWS Shield để chống tấn công DDoS.
  - Tích hợp với AWS Direct Connect giúp doanh nghiệp kết nối trực tiếp với AWS mà không cần qua Internet.
- AWS Global Accelerator
  - Tối ưu hóa lưu lượng truy cập bằng cách tự động chuyển hướng đến endpoint nhanh nhất.
  - Giúp cải thiện hiệu suất cho ứng dụng quốc tế.
## 2. Mô hình chia sẻ trách nhiệm của AWS (AWS Shared Responsibility Model)
### Trách nhiệm của AWS (Security of the Cloud)
- AWS chịu trách nhiệm cho cơ sở hạ tầng toàn cầu: Region, các Edge Locations)
- AWS kiểm soát, bảo vệ các trung tâm dữ liệu, là nơi mà lưu trữ các dữ liệu của bạn
- AWS chịu trách nhiệm bảo trì hệ thống mạng, máy chủ, các thành phần: máy phát điện, hệ thống UPS, hệ thống điều hoà, hệ thống PCCC, ....
- AWS chịu trách nhiệm cho quản trị toàn bộ các dịch vụ như RDS, S3, ECS, Lambda, vá lỗi của các máy chủ vật lý và các điểm truy cập dữ liệu.
### Trách nhiệm của khách hàng (Security in the Cloud)
- Chịu trách nhiệm cho việc quản trị các dữ liệu ứng dụng, bao gồm các tuỳ chọn mã hoá.
- Chịu trách nhiệm cho việc bảo mật tài khoản, các API calls, thông tin đăng nhập, các cấu hình bảo mật hạn chế cho các máy chủ đi ra internet từ VPC.
- Chịu trách nhiệm cho quản trị hệ điều hành trong các máy chủ EC2 instances, như cập nhật các bản vá lỗi để đảm bảo bảo mật.
- Chịu trách nhiệm cho việc bảo mật ứng dụng, quản lý định danh và truy cập các tài nguyên trong tài khoản AWS của bạn.
- Chịu trách nhiệm cho bảo mật, quản trị các luồng truy cập vào ra trong hệ thống mạng ảo VPC, bao gồm cấu hình security group, tường lửa Firewall.
- Chịu trách nhiệm cho các code ứng dụng, cài đặt phần mềm...
### Sự khác biệt giữa dịch vụ Managed và Unmanaged
- Dịch vụ được quản lý (Managed Services)
  - AWS chịu trách nhiệm bảo mật nhiều hơn.
  - Ví dụ: S3, DynamoDB, RDS, Lambda – AWS bảo trì hạ tầng, khách hàng chỉ quản lý dữ liệu và quyền truy cập.
- Dịch vụ không được quản lý (Unmanaged Services)
  - Khách hàng chịu trách nhiệm nhiều hơn.
  - Ví dụ: EC2, VPC, EKS – khách hàng phải cài đặt, cập nhật bảo mật, cấu hình tường lửa.
### Các dịch vụ chính trong AWS
- Computer: EC2, Lambda, Elastic Beenstalk
- Storage: S3, EBS, EFS, FSx, Storage Gateway
- Databases: RDS, DynamoDB, Redshift
- Networking: VPCs, Direct Connect, Route 53, API Gateway, AWS Global Accelerator.
### Các trụ cột của Well-Architected Framework  

| **Trụ cột** | **Mô tả** | **Ví dụ** |
|------------|----------|-----------|
| **Operational Excellence** *(Vận hành xuất sắc)* | Tối ưu hóa quy trình vận hành, tự động hóa, giám sát hệ thống. | Dùng **AWS CloudWatch, AWS Config** để giám sát và phản hồi sự cố. |
| **Security** *(Bảo mật)* | Bảo vệ dữ liệu, quyền truy cập, mã hóa, tuân thủ tiêu chuẩn bảo mật. | Sử dụng **IAM, MFA, KMS, AWS WAF, Shield** để bảo mật. |
| **Reliability** *(Khả năng chịu lỗi)* | Đảm bảo hệ thống ổn định, có khả năng tự phục hồi, sao lưu dự phòng. | Dùng **Auto Scaling, Multi-AZ, S3 Versioning, RDS Backup**. |
| **Performance Efficiency** *(Hiệu suất tối ưu)* | Lựa chọn tài nguyên phù hợp, cân bằng tải, tối ưu mã nguồn. | Dùng **AWS Lambda, Auto Scaling, Amazon CloudFront** để cải thiện hiệu suất. |
| **Cost Optimization** *(Tối ưu chi phí)* | Quản lý chi phí, sử dụng tài nguyên hợp lý, tránh lãng phí. | Sử dụng **AWS Cost Explorer, Reserved Instances, Spot Instances** để tiết kiệm chi phí. |
| **Sustainability** *(Bền vững)* | Giảm thiểu tác động môi trường, sử dụng năng lượng hiệu quả. | Tận dụng **serverless, tối ưu lưu trữ, dùng kiến trúc cloud-native**. |

---

### Công cụ hỗ trợ kiến trúc tối ưu trên AWS  

| **Công cụ** | **Chức năng** |
|------------|--------------|
| **AWS Well-Architected Tool** | Đánh giá kiến trúc theo 6 trụ cột. |
| **AWS Trusted Advisor** | Gợi ý tối ưu bảo mật, hiệu suất, chi phí. |
| **AWS CloudWatch** | Giám sát hiệu suất và cảnh báo sự cố. |
| **AWS Security Hub** | Kiểm tra và đề xuất cải thiện bảo mật. |

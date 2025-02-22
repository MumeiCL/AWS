# 
## Vertical Scaling
**Vertical Scaling (Scaling Up/Down) là quá trình tăng (Scaling Up) hoặc giảm (Scaling Down) tài nguyên của một máy chủ bằng cách nâng cấp CPU, RAM, Storage, v.v.Thay vì thêm nhiều máy chủ, hệ thống chỉ dùng một máy chủ mạnh hơn.**
### Cách hoạt động
- Khi tải tăng, nâng cấp instance lên loại mạnh hơn (ví dụ: từ t2.micro → t3.large).
- Khi tải giảm, hạ cấp instance xuống loại nhỏ hơn để tiết kiệm chi phí.
- Không thay đổi số lượng máy chủ, chỉ nâng cấp hoặc hạ cấp cấu hình.
- Ví dụ:
  - EC2 Instance Resize: Chuyển đổi instance từ loại nhỏ sang loại lớn hơn.
  - Amazon RDS Instance Upgrade: Nâng cấp database server lên phiên bản mạnh hơn.
  - EBS Volume Resize: Tăng dung lượng ổ đĩa cho hệ thống lưu trữ.
  - Elasticache Scaling: Nâng cấp Redis/Memcached lên loại máy chủ lớn hơn.
### Ưu điểm
- Đơn giản: Không cần Load Balancer hay nhiều máy chủ.
- Dễ triển khai: Không cần thay đổi kiến trúc ứng dụng.
- Tối ưu hiệu suất nhanh chóng: Chỉ cần nâng cấp phần cứng.
### Nhược điểm
- Giới hạn tài nguyên: AWS có giới hạn cho từng loại instance. Ví dụ, EC2 lớn nhất là m7g.metal hoặc x2iedn.32xlarge.
- Downtime có thể xảy ra: Khi thay đổi loại instance, hệ thống có thể phải khởi động lại.
- Không linh hoạt khi mở rộng lớn: Nếu tải tiếp tục tăng, có thể phải chuyển sang Horizontal Scaling.

## Horizontal Scaling
**Horizontal Scaling (Scaling Out/In) là quá trình tăng (Scaling Out) hoặc giảm (Scaling In) số lượng máy chủ (instances/nodes) để xử lý tải tăng hoặc giảm.Mô hình này thường dùng nhiều máy chủ nhỏ hơn, thay vì một máy chủ lớn.**
### Cách hoạt động
- Khi tải tăng, hệ thống tạo thêm máy chủ để chia sẻ công việc.
- Khi tải giảm, giảm bớt số máy chủ để tiết kiệm tài nguyên.
- Dữ liệu và công việc thường được phân phối giữa nhiều máy chủ thông qua Load Balancer.
- Ví dụ:
  -  EC2 Auto Scaling Group: Thêm hoặc giảm số lượng EC2 instances tự động.
  - Amazon ECS / EKS Auto Scaling: Tự động mở rộng container instances.
  - DynamoDB Global Tables: Dữ liệu được chia nhỏ và phân phối giữa nhiều node.
  - Amazon RDS Read Replicas: Sử dụng nhiều bản sao database để giảm tải trên primary database.
### Ưu điểm
- Khả năng mở rộng vô hạn: Có thể thêm bao nhiêu máy chủ tùy ý.
- Tính sẵn sàng cao: Nếu một máy chủ hỏng, hệ thống vẫn hoạt động bình thường.
- Hiệu suất cao: Có thể xử lý nhiều request hơn bằng cách phân tán tải.
- Tối ưu chi phí: Chỉ thêm máy chủ khi cần, tiết kiệm chi phí khi tải thấp.
### Nhược điểm:
- Cần Load Balancer: Để phân phối tải giữa nhiều máy chủ.
- Độ phức tạp cao: Cần đồng bộ dữ liệu và quản lý nhiều máy chủ.
- Yêu cầu kiến trúc phân tán: Ứng dụng cần thiết kế để hoạt động trên nhiều máy chủ.
##  Launch Template và Launch Configuration
###  Launch Template (Recommended)
- Hỗ trợ nhiều phiên bản (versioning) → Dễ dàng cập nhật cấu hình mà không cần tạo mới.
- Hỗ trợ Spot Instances → Tiết kiệm chi phí bằng cách sử dụng Spot Pricing.
- Cho phép EC2 Fleet → Kết hợp nhiều loại instance để tối ưu hiệu suất.
- Cấu hình chi tiết hơn → Bao gồm IAM Role, Network Interface, Storage, Placement Group, v.v.
- Hỗ trợ Advanced Features như:
  - Metadata Options (bảo mật dữ liệu instance)
  - Mixed Instance Policies (mix nhiều loại EC2 instance)
-Cấu trúc chính của Launch Template:
  - AMI ID (Image của hệ điều hành)
  - Instance Type (Loại EC2: t3.micro, m5.large, c6g.xlarge, v.v.)
  - Key Pair (SSH Key)
  - Security Group (Quy tắc firewall)
  - IAM Role (Quyền của instance)
  - Network Configuration (VPC, Subnet, Public IP, v.v.)
  - Storage (Loại ổ đĩa, dung lượng)
  - User Data (Script chạy khi khởi động)
- Ưu điểm 
  - Hỗ trợ nhiều phiên bản (versions) để dễ dàng cập nhật.
  - Cho phép Spot Instances để tối ưu chi phí.
  - Hỗ trợ EC2 Fleet, giúp mix nhiều loại instance khác nhau.
### Launch Configuration
- Tương tự Launch Template nhưng không hỗ trợ nhiều phiên bản (versions).
- Không hỗ trợ Spot Instances và EC2 Fleet.
- AWS đề xuất sử dụng Launch Template thay vì Launch Configuration.
## Auto Scaling Policies
**AWS Auto Scaling sử dụng Scaling Policies để tự động mở rộng (scale out) hoặc thu nhỏ (scale in) các EC2 instances trong Auto Scaling Group (ASG) nhằm tối ưu tài nguyên và chi phí.**
### Target Tracking Scaling
- Tự động điều chỉnh số lượng EC2 instance dựa trên một chỉ số mục tiêu.
- AWS sử dụng thuật toán Machine Learning để tối ưu scaling.
- Phù hợp với tải không dự đoán trước, ví dụ: Website, API server.
- Ví dụ:
  - CPU Utilization < 50% → Giảm instance
  - CPU Utilization > 70% → Tăng thêm instance
- Hỗ trợ Predefined Metrics như ASGAverageCPUUtilization, ALBRequestCountPerTarget, v.v.
- Có thể dùng Custom Metrics từ CloudWatch để mở rộng khả năng kiểm soát.
### Step Scaling
- Dựa trên CloudWatch Alarms để điều chỉnh số lượng instances theo mức độ tải.
- Khi vượt ngưỡng, ASG sẽ tăng hoặc giảm instance theo bước (step).
- Phù hợp với tải biến đổi theo mô hình dự đoán trước.
- Ví dụ:
  - CPU > 70%	→ Thêm 2 instances
  - CPU > 90%	→ Thêm 4 instances
  - CPU < 30%	→ Giảm 2 instances
### Scheduled Scaling
- Tự động scale dựa trên thời gian định sẵn (cron job).
- Phù hợp với hệ thống có tải dự đoán trước, ví dụ:
  - Tăng instances vào ban ngày (8:00 AM).
  - Giảm instances vào ban đêm (10:00 PM).
### Kết hợp Scaling Policies
- AWS hỗ trợ kết hợp nhiều Scaling Policies để tối ưu hiệu suất:
  - Target Tracking Scaling để tự động scale theo tải thực tế.
  - Step Scaling để phản ứng nhanh với các đột biến tải.
  - Scheduled Scaling để tối ưu chi phí khi có mô hình tải cố định.
- Ví dụ:
  - Ban ngày (9:00 AM - 5:00 PM) → Dùng Scheduled Scaling để tăng instance.
  - Ban đêm (5:00 PM - 9:00 AM) → Giảm instance về mức tối thiểu.
  - Khi CPU vượt quá 70% → Step Scaling để phản ứng nhanh với đột biến.
  - Khi CPU trung bình giữ ở mức 50% → Target Tracking Scaling để tự động điều chỉnh.

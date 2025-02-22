# Elastic Compute Cloud EC2
**Amazon EC2 là dịch vụ cung cấp máy chủ ảo (instances) trên đám mây của AWS. Với EC2, bạn có thể khởi tạo, cấu hình và vận hành máy chủ trong thời gian ngắn, trả phí theo mô hình “trả tiền theo mức sử dụng”**
## Tính năng và lợi ích chính
### Tính co giãn (Elasticity)
- Dễ dàng tăng giảm tài nguyên máy chủ (CPU, RAM) để đáp ứng nhu cầu của ứng dụng.
- Hỗ trợ Auto Scaling: tự động mở rộng hoặc thu hẹp số lượng EC2 Instances dựa trên các chỉ số như CPU Usage, Network I/O, v.v.
### Đa dạng loại máy chủ (Instance Types)
- EC2 cung cấp nhiều loại máy chủ tối ưu cho các nhu cầu khác nhau:
  - General Purpose (T, M) – Cân bằng CPU, RAM, phù hợp web server, app server.
  - Compute Optimized (C) – Tối ưu CPU cho các tác vụ tính toán cao (batch processing, HPC).
  - Memory Optimized (R, X, Z) – Tối ưu RAM cho cơ sở dữ liệu, in-memory caching.
  - Storage Optimized (I, D, H) – Tối ưu I/O cho hệ thống xử lý dữ liệu lớn, NoSQL DB.
  - Accelerated Computing (P, G, F) – Tích hợp GPU, FPGA cho machine learning, đồ họa, AI.
### Linh hoạt hệ điều hành và ứng dụng
- Chọn Amazon Machine Image (AMI) để cài đặt sẵn hệ điều hành (Linux, Windows, MacOS) và phần mềm (LAMP, .NET, v.v.).
- Tạo Custom AMI để nhân bản cấu hình máy chủ theo ý muốn.
### Tích hợp chặt chẽ với các dịch vụ AWS khác
- Amazon VPC: Tạo mạng ảo riêng, kiểm soát IP, firewall.
- Amazon EBS: Ổ đĩa khối (block storage) cho EC2.
- Amazon S3: Lưu trữ dữ liệu lớn, backup.
- AWS IAM: Quản lý quyền truy cập cho EC2.
### Chi phí sử dụng
- On-Demand Instances: trả tiền theo giờ hoặc theo giây
  - Chi phí thấp và linh hoạt khi không cần phải thanh toán trả trước hoặc cam kết sử dụng dài hạn
  - Phù hợp các ứng dụng có khối lượng công việc ngắn hạn, tăng đột biến hoặc không thể đoán trước mà không thể bị gián đoạn trong hoạt động.
  - Các ứng dụng được phát triển hoặc trong quá trình thử nghiệm trong Amazon EC2 lần đầu tiên.
- Reserved Instances: Trả tiền thuê trong 1 hoặc 3 năm
  - Các ứng dụng đã chạy ổn định hoặc nhu cầu sử dụng được dự báo trước.
  - Các ứng dụng đòi hỏi có dung lượng lưu trữ.
  - Có thể trả tiền trước
- Spot Instances: Mua dung lượng chưa sử dụng với mức chiết khấu lên tới 90%
  - Trường hợp khẩn cấp cần phải dùng một lượng lớn tài nguyên máy tính để tính toán.
  - Các ứng dụng có khả năng linh hoạt trong thời gian bắt đầu chạy và kết thúc
- Dedicated Instances: Một máy chủ EC2 vật lý riêng, chi phí cao nhất.
## Security Groups
**Security Groups (SG) là tường lửa ảo điều khiển luồng traffic đến và đi từ EC2 instances. Nó giúp bảo vệ tài nguyên bằng cách chỉ cho phép các kết nối cụ thể.**

**Đặc điểm chính của Security Groups**
- Kiểm soát Inbound & Outbound Rules
  - Inbound Rules: Xác định traffic nào được phép đi vào instance.
  - Outbound Rules: Xác định traffic nào được phép đi ra từ instance.
- Hoạt động theo mô hình "Allow"
  - Mặc định, tất cả traffic bị từ chối.
  - Chỉ những traffic được cấu hình Allow trong rules mới được phép.
- Gắn với instance, không phải subnet
  - Mỗi EC2 instance có thể gán nhiều Security Groups.
  - Một Security Group có thể dùng chung cho nhiều instance.
- Không hỗ trợ "Deny Rules"
  - AWS Security Groups chỉ có thể cho phép (Allow), không thể chặn (Deny) trực tiếp như Network ACLs.
## Bootstrap Scripts
**Bootstrap Scripts là tập lệnh chạy tự động khi một EC2 instance khởi động lần đầu tiên. Nó được sử dụng để cấu hình hệ thống, cài đặt phần mềm hoặc thực hiện các tác vụ cần thiết trước khi instance sẵn sàng.**
- Script được cung cấp trong "User Data" khi tạo instance.
- Chỉ chạy một lần trong lần boot đầu tiên (mặc định).
- Chạy với quyền root trên hệ điều hành Linux hoặc Windows.
- Có thể viết bằng Bash, PowerShell, hoặc Cloud-Init.
## EC2 Metadata 
**là thông tin về instance mà bạn có thể truy xuất từ bên trong chính instance đó. Nó cung cấp dữ liệu như Instance ID, Public IP, Security Groups, IAM Role, v.v.**
- Khi khởi tạo (Launching) một EC2 instance, mặc định Metadata Version là V2 Only (Token Required). Vì vậy, cần có TOKEN trong lệnh nếu muốn lấy thông tin METADATA trong EC2:
```
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/
```
- Tuy nhiên, có thể chọn Metadata Version là V1 and V2 (token optional) khi khởi tạo (Launching) một EC2 instance. Điều này cho phép bạn không cần TOKEN trong lệnh nếu muốn lấy thông tin METADATA trong EC2:

`curl http://169.254.169.254/latest/meta-data/`
## Placement Groups
**Cluster Placement Group**
- Dùng khi cần độ trễ thấp và băng thông cao giữa các EC2 instances.
- Các instances được đặt rất gần nhau trong cùng một Availability Zone (AZ).
- Phù hợp với High-Performance Computing (HPC), Big Data, AI, Machine Learning.
- Nhược điểm: Nếu AZ bị lỗi, toàn bộ instances có thể bị ảnh hưởng.

**Spread Placement Group**
- Dùng khi cần phân tán instances để tăng tính sẵn sàng.
- Các instances được trải rộng trên nhiều phần cứng vật lý khác nhau.
- Giới hạn: Tối đa 7 instances trên mỗi AZ.
- Phù hợp với ứng dụng quan trọng, yêu cầu high availability (VD: database, critical services).

**Partition Placement Group**
- Dùng khi muốn bảo vệ ứng dụng khỏi lỗi phần cứng.
- Instances được chia thành nhiều Partition, mỗi Partition đặt trên một phần cứng riêng.
- Partition có thể nằm trong một hoặc nhiều AZ.
- Tốt cho Hadoop, Cassandra, HDFS, nơi dữ liệu cần được tách biệt để tránh lỗi toàn bộ hệ thống.

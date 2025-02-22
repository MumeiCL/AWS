# AWS Elastic Block Store (EBS)
**AWS Elastic Block Store (EBS) là một dịch vụ lưu trữ khối (block storage) được thiết kế để sử dụng với Amazon EC2.**
## Đặc điểm
- Được thiết kế cho khối lượng công việc quan trọng
- Tính sẵn sàng cao, tự động sao chép trong một vùng AZ đơn để bảo vệ dữ liệu trong trường hợp ổ cứng máy chủ vật lý bị lỗi.
- Khả năng mở rộng, tự động tăng dung lượng và thay đổi loại ổ đĩa mà không bị gián đoạn (no down time) hoặc ảnh hưởng đến hiệu suât (performance) đối với hệ thống máy chủ EC2 instances đang chạy.
## Các loại ổ đĩa EBS
**IOPS(input/output per second)**

## So sánh các loại ổ đĩa EBS

| Loại Volume   | IOPS tối đa | Băng thông tối đa | Chi phí  | Dùng cho |
|--------------|------------|----------------|--------|----------------------|
| **gp3 (SSD)**  | 16.000     | 1.000 MB/s     | Thấp   | Web server, ứng dụng doanh nghiệp |
| **gp2 (SSD)**  | 16.000     | 250 MB/s       | Trung bình | Web, Boot volumes |
| **io2 (SSD)**  | 256.000    | 4.000 MB/s     | Cao    | Database lớn, SAP HANA |
| **io1 (SSD)**  | 64.000     | 1.000 MB/s     | Cao    | Database quan trọng |
| **st1 (HDD)**  | 500        | 500 MB/s       | Rẻ     | Big Data, Streaming |
| **sc1 (HDD)**  | 250        | 250 MB/s       | Rẻ nhất | Lưu trữ dài hạn |

## EBS Volume
**Amazon Elastic Block Store (EBS) Volume là ổ đĩa lưu trữ khối có thể gắn vào một EC2 Instance. Nó hoạt động tương tự như ổ cứng vật lý, nhưng được lưu trữ trên nền tảng đám mây của AWS.**
- Dữ liệu được lưu trữ liên tục, ngay cả khi EC2 instance bị tắt hoặc khởi động lại.
- Mỗi EBS Volume chỉ tồn tại trong một Availability Zone (AZ) và không thể gắn trực tiếp vào EC2 Instance ở AZ khác.
- Hỗ trợ sao lưu và phục hồi dữ liệu thông qua EBS Snapshots.
- Có thể gắn nhiều volume vào một EC2 instance.
## EBS Snapshot
**EBS Snapshot là bản sao lưu dữ liệu của một EBS Volume tại một thời điểm cụ thể.**
- Snapshot không lưu toàn bộ volume mà chỉ lưu các block thay đổi từ lần snapshot trước (Incremental Backup).
- Snapshot được lưu trữ trong Amazon S3, nhưng không thể truy cập trực tiếp từ S3 (chỉ có thể khôi phục lại volume).
- Có thể sử dụng snapshot để tạo volume mới hoặc sao chép dữ liệu giữa các khu vực (Region, AZs).
- Hỗ trợ mã hóa, nếu volume gốc đã được mã hóa.

**Cách Snapshot hoạt động**
- Lần snapshot đầu tiên: Lưu toàn bộ dữ liệu trên volume.
- Các snapshot tiếp theo: Chỉ lưu dữ liệu thay đổi, giúp tiết kiệm dung lượng.
- Khi xóa snapshot cũ, dữ liệu không bị mất vì AWS giữ lại block dữ liệu cần thiết.
**Lợi ích**
- Tiết kiệm dung lượng: Chỉ lưu trữ dữ liệu thay đổi, giảm chi phí lưu trữ.
- Nhanh chóng phục hồi: Tạo volume mới từ snapshot dễ dàng.
- Di chuyển dữ liệu: Chuyển volume giữa Region/AZ bằng snapshot.
- Bảo vệ dữ liệu: Dữ liệu được lưu trữ trong Amazon S3 để đảm bảo an toàn.

**Các tình huống sử dụng EBS Snapshot**
- Sao lưu dữ liệu định kỳ: Dùng snapshot để đảm bảo dữ liệu quan trọng không bị mất.
- Di chuyển dữ liệu: Tạo snapshot, copy sang region khác, khôi phục volume mới.
- Tạo AMI (Amazon Machine Image): Snapshot giúp lưu trạng thái hệ thống để khởi tạo instance mới.

## EC2 Hibernation
**EC2 Hibernation (Ngủ đông) là một tính năng của AWS EC2 giúp lưu trạng thái bộ nhớ RAM của máy ảo (EC2 Instance) vào ổ đĩa EBS để khi khởi động lại, hệ thống có thể tiếp tục làm việc mà không cần khởi động lại từ đầu. Tương tự như chế độ Hibernate trên máy tính cá nhân, giúp tiết kiệm thời gian khi khởi động lại.**
![image](https://github.com/user-attachments/assets/ee0ed72c-7199-45b7-865f-6671493163a0)

**Cách hoạt động của Hibernation trong EC2**
- Khi bật chế độ Hibernation, EC2 lưu toàn bộ dữ liệu trong RAM vào EBS.
- Instance được tắt nhưng ổ đĩa gốc (Root Volume) và RAM vẫn giữ nguyên trạng thái.
- Khi bật lại, dữ liệu RAM được nạp lại vào bộ nhớ và hệ thống hoạt động như trước khi ngủ đông.
- Thời gian khởi động nhanh hơn vì không cần chạy lại các ứng dụng từ đầu.

**Chỉ hỗ trợ trên một số loại Instance:**
- C5, C6i, M5, M6i, R5, R6i và các biến thể metal của chúng.
- Dung lượng RAM tối đa 150 GB.
- Phải sử dụng EBS-Backed Volume (ổ EBS làm ổ gốc).
- Hệ điều hành hỗ trợ:
  - Amazon Linux 2
  - Ubuntu 18.04 trở lên
  - Windows Server 2016 trở lên
- Yêu cầu về ổ đĩa:
  - Ổ đĩa EBS phải được mã hóa.
  - Kích thước Root Volume phải đủ lớn để lưu toàn bộ dữ liệu trong RAM.
#  Elastic File System (EFS)
**Amazon EFS là một dịch vụ lưu trữ tệp được quản lý hoàn toàn trên AWS, cung cấp hệ thống tệp có khả năng mở rộng cao, có thể truy cập đồng thời từ nhiều EC2 instance.**
## Đặc điểm chính của Amazon EFS
- Lưu trữ linh hoạt: Tự động mở rộng hoặc thu nhỏ dung lượng theo nhu cầu sử dụng.
- Truy cập đồng thời: Nhiều EC2 instance có thể truy cập cùng một hệ thống tệp một cách đồng thời.
- Hiệu suất cao: Hỗ trợ throughput cao, độ trễ thấp, phù hợp cho các ứng dụng xử lý dữ liệu lớn.
- Tích hợp với AWS: Có thể kết nối với Lambda, ECS, EKS, AWS Backup và nhiều dịch vụ AWS khác.
- Bảo mật mạnh mẽ: Hỗ trợ mã hóa dữ liệu khi truyền và lưu trữ, tích hợp với IAM và Security Groups để kiểm soát quyền truy cập.
- Hỗ trợ NFS: Hỗ trợ giao thức NFSv4.1 và NFSv4.2 để kết nối với các hệ thống Linux.

## Chế độ lưu trữ và chế độ hiệu suất
|Chế độ	|Mô tả	|Chi phí|
|--------------|------------|----------------|
|Standard	|Lưu trữ dữ liệu thường xuyên truy cập, độ trễ thấp, hiệu suất cao.|	Cao hơn|
|Infrequent Access (IA)|	Lưu trữ dữ liệu ít truy cập, chi phí thấp hơn nhưng vẫn có độ trễ thấp.|	Rẻ hơn|

|Chế độ hiệu suất|	Mô tả|
|--------------|------------|
|General Purpose (Mặc định)|	Phù hợp với hầu hết các ứng dụng, tối ưu hóa cho độ trễ thấp.|
|Max I/O|	Hỗ trợ song song truy cập cao, phù hợp với khối lượng công việc lớn.|

## Ứng dụng của Amazon EFS
- Lưu trữ dữ liệu cho nhiều EC2 instance truy cập chung.
- Xây dựng hệ thống web server với nhiều máy chủ có chung thư mục.
- Dùng làm NAS (Network Attached Storage) trong môi trường cloud.
- Lưu trữ và phân tích dữ liệu lớn (Big Data, Machine Learning).
- Dùng với container (ECS, EKS, Kubernetes).

# Amazon FSx
## Amazon FSx for Windows File Server
**cung cấp dịch vụ lưu trữ file được quản lý hoàn toàn, hỗ trợ giao thức SMB (Server Message Block) và hệ thống file NTFS. Dịch vụ này phù hợp với doanh nghiệp cần môi trường Windows để chia sẻ file, chạy ứng dụng Microsoft SQL Server, hoặc các hệ thống ERP.**
| Feature            | FSx for Windows                                      | EFS (Elastic File System)                        |
|--------------------|------------------------------------------------------|-------------------------------------------------|
| **Protocol**      | Windows Server Message Block (SMB)                   | Network File System (NFS) v4                    |
| **Compatibility** | Windows applications                                 | Unix/Linux systems                             |
| **User Support**  | Active Directory (AD) users                          | Multi-instance sharing                         |
| **Security**      | Access Control Lists (ACLs), groups, security policies | N/A                                             |
| **Additional Features** | Distributed File System (DFS) namespaces, replication | Native file sharing for Unix/Linux             |

## Amazon FSx for Lustre
**Amazon FSx for Lustre là dịch vụ file system hiệu năng cao, được tối ưu cho Machine Learning, AI, Big Data, và HPC (High-Performance Computing).**

# Amazon Machine Images (AMI)
- AMI là một mẫu (template) dùng để tạo EC2 instances.
- Chứa Hệ điều hành (OS), ứng dụng, cấu hình, dữ liệu liên quan.
- Có thể tạo từ EBS Snapshot hoặc Instance Store-backed Image.
- Cho phép sao lưu (backup), nhân bản (replication), triển khai nhanh nhiều EC2 instances.
## Các loại AMI:
- EBS-backed AMI: Sử dụng Amazon EBS làm bộ nhớ gốc.
- Instance Store-backed AMI: Sử dụng Instance Store làm bộ nhớ gốc.
## Thông tin căn bản khi bạn lựa chọn AMI của bạn
- Region
- Operating system
- Architecture (32-bit or 64-bit)
- Cấp các quyền hạn cho phép EC2 truy cập tài nguyên AWS
- Cấu hình lưu trữ cho root device volume
## Instance Store
- Instance Store là bộ nhớ tạm thời (ephemeral storage), lưu trực tiếp trên ổ đĩa vật lý của host.
- Tốc độ cao nhưng mất dữ liệu khi instance bị dừng (stopped) hoặc bị terminate.
- Không thể tạo snapshot, gắn hoặc tháo rời như EBS.
- Phù hợp với cache, buffer, temporary storage.

# AWS Backup
**AWS Backup là một dịch vụ quản lý sao lưu tập trung, giúp tự động hóa việc sao lưu dữ liệu trên các dịch vụ AWS, bao gồm EC2, EBS, RDS, DynamoDB, EFS, FSx và nhiều dịch vụ khác**.
## Tính năng chính 
- Quản lý sao lưu tập trung: Giúp quản lý tất cả các bản sao lưu từ một giao diện duy nhất.
- Tự động hóa chính sách sao lưu: Cho phép thiết lập lịch trình sao lưu theo nhu cầu.
- Hỗ trợ nhiều dịch vụ AWS: EC2, EBS, RDS, DynamoDB, EFS, FSx, VMware, SAP HANA,...
- Tuân thủ và bảo mật: Hỗ trợ mã hóa dữ liệu, kiểm soát truy cập và lưu trữ lâu dài.
- Sao lưu theo vùng (Cross-Region Backup): Cho phép sao lưu dữ liệu giữa các vùng AWS để đảm bảo tính khả dụng cao.
- Sao lưu theo tài khoản (Cross-Account Backup): Bảo vệ dữ liệu bằng cách sao lưu sang tài khoản AWS khác.
- Khả năng phục hồi nhanh chóng: Hỗ trợ khôi phục dữ liệu nhanh chóng khi cần.

## Lợi ích của AWS Backup

### Quản lý tập trung  
- Sử dụng **central backup console** duy nhất.  
- Cho phép tập trung quản lý **backup dữ liệu** từ nhiều dịch vụ AWS và nhiều tài khoản AWS (AWS Accounts).  

### Tự động hóa (Automation)  
- Hỗ trợ tạo **lịch trình backup tự động**.  
- Thiết lập **chính sách lưu giữ** các bản backup trong một thời gian nhất định.  
- **Xóa tự động** các bản backup không còn cần thiết sau thời gian quy định.  

### Tăng cường sự tuân thủ  
- Cho phép thực thi **mã hóa** các bản backup cả khi lưu trữ và trong quá trình truyền tải.  
- Hỗ trợ chính sách linh hoạt để **đáp ứng các tiêu chuẩn tuân thủ**.  
- Chế độ **kiểm tra và báo cáo tập trung** giúp theo dõi các bản backup trên nhiều dịch vụ AWS.

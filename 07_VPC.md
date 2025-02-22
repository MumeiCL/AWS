# Virtual Private Cloud
**AWS VPC (Virtual Private Cloud) là một dịch vụ cung cấp một mạng riêng ảo trong AWS, nơi bạn có thể triển khai các tài nguyên AWS như EC2, RDS, Lambda,... với khả năng kiểm soát mạng linh hoạt, bảo mật cao. Tạo một mạng riêng trong AWS với phạm vi IP do người dùng tự chọn.
Toàn quyền kiểm soát cấu trúc mạng, subnet, route table, security group, v.v.**
## Networking
- AWS VPC cung cấp các thành phần giúp bạn thiết kế và quản lý hệ thống mạng một cách linh hoạt:
  - Subnets: Chia VPC thành các subnet riêng biệt (Public Subnet, Private Subnet).
  - Route Tables: Xác định cách thức định tuyến lưu lượng trong VPC.
  - Internet Gateway (IGW): Cho phép các subnet public truy cập Internet.
  - NAT Gateway: Cho phép các subnet private truy cập Internet một chiều.
  - Elastic IP (EIP): Địa chỉ IP tĩnh công khai có thể gán cho EC2 hoặc NAT Gateway.
## VPNs
- AWS hỗ trợ nhiều giải pháp VPN để kết nối VPC với mạng bên ngoài:
  - AWS Site-to-Site VPN: Kết nối VPC với mạng on-premises.
  - AWS Client VPN: Kết nối từ xa vào VPC bằng VPN client.
  - AWS Direct Connect: Kết nối trực tiếp từ trung tâm dữ liệu đến AWS với băng thông cao.
## NAT Gateway 
**AWS NAT Gateway là một dịch vụ giúp các instance trong Private Subnet có thể gửi yêu cầu ra Internet hoặc đến dịch vụ AWS bên ngoài, nhưng không cho phép kết nối từ bên ngoài vào.**
### Cách hoạt động 
- Khi một instance trong Private Subnet gửi yêu cầu đến Internet:
  - Lưu lượng từ instance sẽ đi qua Route Table, trong đó có một tuyến đường chỉ đến NAT Gateway.
  - NAT Gateway thay thế địa chỉ IP riêng của instance bằng Elastic IP (EIP) của NAT Gateway.
  - Yêu cầu được gửi ra Internet thông qua Internet Gateway (IGW).
  - Khi phản hồi quay lại, NAT Gateway dịch địa chỉ IP công khai trở lại IP riêng của instance và chuyển tiếp phản hồi.
## Network ACL
**AWS Network Access Control List (NACL) là một bộ lọc bảo mật cấp subnet, kiểm soát lưu lượng vào (Inbound) và lưu lượng ra (Outbound) khỏi subnet trong Amazon Virtual Private Cloud (VPC).**
### Chức năng chính của NACL:
- Hoạt động như một firewall stateless ở cấp subnet.
- Kiểm soát truy cập dựa trên danh sách quy tắc (rules) cho phép hoặc từ chối lưu lượng.
- Áp dụng quy tắc theo thứ tự số ưu tiên (rule number) từ thấp đến cao.
- Lưu ý:
  - Mỗi VPC tự động có một NACL mặc định, cho phép toàn bộ lưu lượng.
  - Người dùng có thể tạo custom NACL và gán nó cho một hoặc nhiều subnet.
  - NACL hoạt động stateless: nếu một gói tin vào được cho phép, thì gói tin phản hồi cũng cần một quy tắc outbound riêng để cho phép.
### Cấu trúc 
- Network ACL bao gồm hai danh sách quy tắc chính:
  - Inbound Rules (kiểm soát lưu lượng vào subnet).
  - Outbound Rules (kiểm soát lưu lượng ra khỏi subnet).
- Mỗi quy tắc trong NACL có các thông tin:
  - Rule Number: Độ ưu tiên của quy tắc (số thấp có ưu tiên cao hơn).
  - Protocol: Loại giao thức (TCP, UDP, ICMP,...).
  - Port Range: Phạm vi cổng (22 cho SSH, 80 cho HTTP,...).
  - Source/Destination: Địa chỉ IP nguồn hoặc đích.
  - Allow/Deny: Hành động chấp nhận hoặc từ chối.
 
## VPC Endpoint 
**VPC Endpoint là một dịch vụ giúp kết nối Amazon VPC với các dịch vụ AWS (như S3, DynamoDB, SNS, SQS...) mà không cần sử dụng Internet, NAT Gateway, VPN hoặc Direct Connect.**
- Lợi ích chính của VPC Endpoint:
  - Bảo mật cao hơn: Dữ liệu không đi qua Internet.
  - Giảm chi phí: Không cần sử dụng NAT Gateway hoặc Internet Gateway.
  - Hiệu suất tốt hơn: Kết nối nội bộ trong mạng AWS, giảm độ trễ.
  - Ví dụ: Thay vì truy cập S3 thông qua Internet, bạn có thể sử dụng VPC Endpoint để truy cập S3 trực tiếp từ VPC.
### Phân loại  
| **Loại**              | **Dịch vụ áp dụng**                        | **Cách kết nối**                                     |
|----------------------|--------------------------------|------------------------------------------------|
| **Interface Endpoint** | Hầu hết các dịch vụ AWS như S3, DynamoDB, SNS, SQS, EC2... | Sử dụng **AWS PrivateLink**, tạo một ENI (Elastic Network Interface) trong VPC |
| **Gateway Endpoint**   | Chỉ dành cho **S3** và **DynamoDB**        | Tạo một entry trong **Route Table** của VPC    |

### So sánh Interface Endpoint và Gateway Endpoint  

| **Tiêu chí**          | **Interface Endpoint** | **Gateway Endpoint** |
|----------------------|----------------------|----------------------|
| **Dịch vụ hỗ trợ**  | Hầu hết dịch vụ AWS | Chỉ S3 và DynamoDB |
| **Kết nối**        | Tạo **Elastic Network Interface (ENI)** trong subnet | Cấu hình trong **Route Table** |
| **Chi phí**        | Trả phí theo lượng request | Miễn phí |
| **Cách sử dụng**   | Yêu cầu thay đổi DNS hoặc sử dụng Private IP | Tự động áp dụng khi thêm vào Route Table |
| **Bảo mật**        | Có thể gán Security Group | Kiểm soát bằng Route Table và IAM Policy |

## VPC Peering
**VPC Peering là một kết nối mạng cho phép bạn kết nối hai Amazon Virtual Private Clouds (VPCs) với nhau để chúng có thể giao tiếp trực tiếp bằng private IP mà không cần đi qua Internet, VPN, hay Direct Connect.**
### Đặc điểm
- Kết nối ngang hàng (Peer-to-Peer): Không có máy chủ trung gian, giúp giảm độ trễ.
- Bảo mật cao: Giao tiếp hoàn toàn qua private IP, không qua Internet.
- Không cần NAT, VPN hay Gateway: Giúp đơn giản hóa kết nối giữa các VPC.
- Hỗ trợ liên vùng (Inter-Region Peering): Kết nối giữa các VPC ở các khu vực (AWS Regions) khác nhau.
- Không hỗ trợ chuyển tiếp (No Transitive Peering): Nếu VPC A kết nối với VPC B, và VPC B kết nối với VPC C, thì VPC A không thể giao tiếp với VPC C qua VPC B.
## AWS PrivateLink
**AWS PrivateLink là dịch vụ giúp bạn kết nối các dịch vụ AWS hoặc dịch vụ của bên thứ ba một cách an toàn qua private IP, mà không cần đi qua Internet. Nó được sử dụng để truy cập các dịch vụ trên VPC khác hoặc AWS Services mà vẫn đảm bảo tính bảo mật cao.**
### Cấu trúc của AWS PrivateLink
- AWS PrivateLink hoạt động dựa trên 3 thành phần chính:
  - Service Provider (Nhà cung cấp dịch vụ): Là VPC chứa dịch vụ cần cung cấp (ví dụ: API, Database).
  - Service Consumer (Người sử dụng dịch vụ): VPC cần kết nối đến dịch vụ qua PrivateLink.
  - VPC Endpoint (Interface Endpoint): Một Network Interface (ENI) trong VPC Consumer giúp giao tiếp với dịch vụ.
  - Lưu ý: PrivateLink chỉ hỗ trợ TCP, không hỗ trợ UDP.
### Ưu điểm của AWS PrivateLink
- Bảo mật cao: Không đi qua Internet, giúp giảm rủi ro tấn công.
- Hiệu suất tốt: Giảm độ trễ do sử dụng mạng AWS backbone.
- Đơn giản hóa networking: Không cần NAT, VPN hay peering.
- Kết nối liên tài khoản và liên VPC: Giúp truy cập dịch vụ giữa các tài khoản AWS khác nhau.
- Hỗ trợ nhiều dịch vụ AWS: Như S3, EC2, DynamoDB, KMS, v.v.
## VPN CloudHub
**AWS VPN CloudHub là một tính năng của AWS Site-to-Site VPN cho phép bạn kết nối nhiều văn phòng hoặc trung tâm dữ liệu (on-premises) với nhau thông qua AWS Virtual Private Gateway. Nó hoạt động theo mô hình hub-and-spoke, giúp các văn phòng có thể giao tiếp với nhau mà không cần kết nối VPN riêng lẻ giữa từng cặp.**
### Cách hoạt động
- Mỗi chi nhánh (on-premises) thiết lập một VPN connection đến AWS Virtual Private Gateway.
- Virtual Private Gateway đóng vai trò trung tâm (hub), giúp các chi nhánh (spokes) giao tiếp với nhau.
- VPN CloudHub sử dụng BGP (Border Gateway Protocol) để quảng bá các tuyến đường (routes) giữa các chi nhánh.
### Ưu điểm
- Không cần kết nối riêng giữa các chi nhánh: Mỗi chi nhánh chỉ cần một VPN đến AWS.
- Dễ mở rộng: Có thể thêm nhiều chi nhánh mà không phải thiết lập nhiều kết nối VPN phức tạp.
- Chi phí thấp: Sử dụng AWS làm trung gian thay vì phải thuê đường truyền riêng (MPLS).
- Độ tin cậy cao: Tận dụng tính sẵn sàng cao của AWS.
## Transit Gateway
**AWS Transit Gateway là một dịch vụ mạng trung tâm giúp kết nối nhiều VPCs (Virtual Private Clouds), on-premises networks, và AWS Direct Connect hoặc AWS Site-to-Site VPN một cách đơn giản và hiệu quả. Nó hoạt động theo mô hình hub-and-spoke, giúp quản lý kết nối giữa nhiều mạng dễ dàng hơn so với việc thiết lập các kết nối VPN hoặc peering riêng lẻ.**
### Cách Hoạt Động
- AWS Transit Gateway hoạt động như một hub trung tâm để kết nối nhiều VPC, mạng on-premises và dịch vụ AWS khác.
- Thay vì tạo nhiều kết nối riêng giữa các VPC (VPC Peering), các mạng chỉ cần kết nối đến Transit Gateway.
- Hỗ trợ BGP (Border Gateway Protocol) giúp trao đổi định tuyến động.
- Hỗ trợ multicast, cho phép gửi dữ liệu đến nhiều điểm đích cùng lúc.
### Ưu điểm
- Quản lý mạng tập trung: Dễ dàng kết nối và quản lý nhiều VPCs mà không cần thiết lập nhiều kết nối riêng.
- Giảm độ phức tạp: Không cần tạo quá nhiều VPC Peering.
-Tăng cường bảo mật: Có thể áp dụng chính sách kiểm soát truy cập bằng AWS Route Tables.
- Tích hợp với Direct Connect & VPN: Cho phép kết nối trực tiếp với các mạng on-premises.
- Hỗ trợ định tuyến động: BGP giúp các mạng trao đổi thông tin tuyến đường tự động.

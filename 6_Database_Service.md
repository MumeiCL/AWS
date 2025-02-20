# AWS Database Service 

## AWS RDS  
Amazon RDS (Relational Database Service) là dịch vụ **quản lý cơ sở dữ liệu quan hệ** trên AWS, giúp dễ dàng thiết lập, vận hành và mở rộng cơ sở dữ liệu trong đám mây.

## Các hệ quản trị cơ sở dữ liệu hỗ trợ  
AWS RDS hỗ trợ nhiều hệ quản trị CSDL phổ biến, bao gồm:  
- **Amazon Aurora** (MySQL & PostgreSQL compatible)  
- **MySQL**  
- **PostgreSQL**  
- **MariaDB**  
- **Oracle Database**  
- **Microsoft SQL Server**  

## Các tính năng chính của AWS RDS  

### **Quản lý tự động**  
- **Triển khai dễ dàng**: Chỉ cần vài bước để tạo và cấu hình CSDL.  
- **Cập nhật phần mềm tự động**: RDS có thể cập nhật và vá lỗi tự động.  
- **Sao lưu và phục hồi**: Hỗ trợ **backup tự động** và **point-in-time recovery**.  

### **Hiệu suất và mở rộng**  
- **Scaling linh hoạt**: Có thể tăng/giảm kích thước CSDL mà không gián đoạn.  
- **Tích hợp read replicas**: Tạo **bản sao đọc (Read Replicas)** để tăng hiệu suất.  
- **Storage tự động mở rộng**: Hỗ trợ **Auto Scaling** dung lượng lưu trữ.  

### **Bảo mật cao**  
- **Mã hóa dữ liệu** bằng AWS KMS (cả khi lưu trữ và truyền tải).  
- **Quản lý quyền truy cập** bằng AWS IAM và Security Groups.  
- **Tích hợp VPC** để kiểm soát mạng nội bộ và bảo mật.  

### **Khả năng chịu lỗi & HA (High Availability)**  
- **Multi-AZ Deployment**: Triển khai đa vùng để đảm bảo tính khả dụng cao.  
- **Automated Failover**: Tự động chuyển đổi khi có lỗi.  
- **Read Replicas**: Phân phối tải để tối ưu truy vấn đọc.  

## Khi nào nên sử dụng AWS RDS?  
**AWS RDS chủ yếu được thiết kế cho OLTP (Online Transaction Processing) workloads, vì nó hỗ trợ các hệ quản trị cơ sở dữ liệu quan hệ như MySQL, PostgreSQL, SQL Server, MariaDB, và Oracle.**
- AWS RDS phù hợp với OLTP
  - Xử lý giao dịch nhanh với khả năng đọc/ghi tốc độ cao.
  - Hỗ trợ ACID (Atomicity, Consistency, Isolation, Durability) để đảm bảo tính toàn vẹn dữ liệu.
  - Replication với Read Replicas giúp cải thiện hiệu suất đọc.
  - Tự động sao lưu & phục hồi, giúp bảo vệ dữ liệu trong các hệ thống giao dịch.
  - Multi-AZ Deployment hỗ trợ tính khả dụng cao, giảm downtime.
- AWS RDS không phải là lựa chọn tối ưu cho OLAP
  - Thiết kế của RDS tập trung vào xử lý giao dịch, không tối ưu cho truy vấn phân tích dữ liệu lớn.
  - Không hỗ trợ columnar storage (lưu trữ dạng cột) như Redshift.
  - OLAP cần xử lý khối lượng lớn dữ liệu theo kiểu batch, trong khi RDS chủ yếu tập trung vào giao dịch nhỏ và nhanh.

## Multi-AZ Deployment trong AWS RDS
**Multi-AZ Deployment là tính năng của Amazon RDS giúp tăng tính sẵn sàng (High Availability - HA) và khả năng chịu lỗi (Fault Tolerance) bằng cách tự động sao chép dữ liệu sang một vùng sẵn sàng (Availability Zone - AZ) khác trong cùng một khu vực (Region).**
### Cách hoạt động
- Khi bật Multi-AZ, AWS RDS sẽ:
  - Tạo một bản sao đồng bộ (Standby Replica) của cơ sở dữ liệu chính trong một AZ khác.
  - Tự động chuyển đổi (Failover) sang Standby Replica nếu máy chủ chính gặp sự cố.
  - Dữ liệu được sao chép đồng bộ giữa máy chủ chính và máy chủ dự phòng.
- Lưu ý: Standby Replica không phục vụ đọc, chỉ được dùng để failover.
### Lợi ích
- Tăng tính sẵn sàng (High Availability)
  - Khi AZ chính gặp sự cố (hardware failure, maintenance, network issues), RDS tự động chuyển sang Standby Replica mà không mất dữ liệu.
  - Giảm downtime xuống dưới 60 giây trong hầu hết trường hợp.
- Bảo vệ dữ liệu với sao lưu đồng bộ
  - Dữ liệu được ghi đồng thời vào cả Primary DB và Standby DB, đảm bảo tính nhất quán.
- Tự động nâng cấp và bảo trì
  - RDS thực hiện update phần mềm và bảo trì trên máy chủ chính mà không ảnh hưởng đến ứng dụng.
  - Khi bảo trì, RDS failover sang Standby Replica để tránh gián đoạn.
## AWS RDS Read Replica
**Read Replica là tính năng trong Amazon RDS cho phép sao chép dữ liệu từ database chính (Primary DB) sang một hoặc nhiều database phụ (Replica DB) theo cơ chế asynchronous replication (không đồng bộ).**
- Read Replica chỉ dùng để đọc, không hỗ trợ ghi.
- Dữ liệu có thể chậm hơn so với Primary DB do cơ chế replication không đồng bộ.
- Một read replica có thể nằm trong các AZ khác nhau.
- Mỗi một read relica có riêng một DNS Endpoin
### Cách hoạt động
- Dữ liệu được sao chép không đồng bộ (Asynchronous Replication) từ Primary DB sang Read Replica.
- Replica chỉ phục vụ truy vấn đọc (SELECT), không thể ghi (INSERT, UPDATE, DELETE).
- Có thể tạo nhiều Read Replica (tối đa 5 Replica trên mỗi Primary DB).
- Nếu cần, có thể nâng cấp Read Replica thành Primary DB khi cần thay thế database chính.
### Lợi ích 
- Mở rộng khả năng đọc (Read Scalability)
  - Ứng dụng có thể phân phối truy vấn đọc giữa nhiều Read Replica để tăng hiệu suất.
  - Phù hợp với hệ thống có nhiều người dùng đọc dữ liệu cùng lúc.
- Giảm tải cho Primary DB
  - Chuyển tải đọc (SELECT) sang Replica giúp Primary DB tập trung vào ghi (INSERT, UPDATE, DELETE).
  - Cải thiện hiệu suất tổng thể của hệ thống.
- Hỗ trợ DR (Disaster Recovery)
  - Nếu Primary DB gặp sự cố, có thể nâng cấp Read Replica thành Primary DB để duy trì hoạt động.

## Aurora
**Amazon Aurora là một dịch vụ cơ sở dữ liệu quan hệ (RDS) được quản lý hoàn toàn bởi AWS, tương thích với MySQL và PostgreSQL.**
### Tính năng nổi bật:
- Hiệu suất cao: Nhanh hơn 5 lần MySQL và 3 lần PostgreSQL.
- Tự động mở rộng: Dữ liệu có thể mở rộng lên 128 TB.
- Sao lưu liên tục: Tích hợp sẵn backup và phục hồi tự động.
- Độ sẵn sàng cao: Tự động nhân bản dữ liệu 6 lần qua 3 AZ.
### Hiệu suất (Performance) của Aurora
- Amazon Aurora tối ưu hóa hiệu suất so với RDS tiêu chuẩn nhờ:
  - Storage ảo hóa: Lưu trữ dữ liệu trên distributed storage với độ trễ thấp.
  - Replication nhanh: Đồng bộ dữ liệu trên 3 AZ, đảm bảo HA (High Availability).
  - Tự động tối ưu hóa truy vấn với Aurora Machine Learning.
  - Tối ưu ghi (Write Optimization): Dữ liệu chỉ ghi một lần, không ghi đè

### Căn bản về Aurora
- Tương thích MySQL và PostgreSQL: Dễ dàng di chuyển từ các database truyền thống.
- Hỗ trợ Auto Scaling: Có thể mở rộng khi nhu cầu tăng cao.
- Tích hợp tốt với AWS Lambda, S3, IAM, CloudWatch.
- Chạy trong Amazon RDS nhưng với kiến trúc hiệu suất cao hơn.
- Cho phép 2 bản copy dữ liệu của bạn được lưu trữ trong mỗi AZ với tối thiểu 3 AZ.

### Khả năng mở rộng của Aurora
- Tự động mở rộng dung lượng (Auto-Scaling Storage):
  - Mở rộng từ 10GB lên 128TB mà không cần cấu hình thủ công.
  - Không bị downtime khi mở rộng.
- Tự động điều chỉnh Read Replica:
  - Hỗ trợ lên đến 15 Aurora Replicas.
  - Tích hợp load balancing để phân phối truy vấn đọc.
- Aurora Global Database:
  - Nhân bản dữ liệu trên nhiều khu vực (Regions).
  - Thời gian sao chép dưới 1 giây, phù hợp với ứng dụng toàn cầu.
  
### Các loại Aurora Replicas
- Aurora Replica (Đồng bộ - Sync Replication)
  - Hỗ trợ 15 bản sao trên nhiều AZ.
  - Tốc độ sao chép cực nhanh (< 100ms).
  - Hỗ trợ Auto Failover khi Primary DB gặp sự cố.
- Read Replica tiêu chuẩn (Không đồng bộ - Async Replication)
  - Giống với MySQL/PostgreSQL Read Replica.
  - Không hỗ trợ Auto Failover.
  - Thích hợp để mở rộng đọc nhưng không thay thế HA.

=> Aurora Replica: Nếu cần High Availability và Auto Failover.
   Read Replica tiêu chuẩn: Nếu chỉ cần mở rộng đọc.
### Aurora Backups
- Continuous Backup (Sao lưu liên tục)
  - Aurora tự động sao lưu dữ liệu mỗi 5 phút vào Amazon S3.
  - Không ảnh hưởng đến hiệu suất hệ thống.
- Point-in-Time Recovery (Khôi phục theo thời điểm)
  - Có thể khôi phục về bất kỳ thời điểm nào trong vòng 35 ngày.
  - Nhanh hơn so với backup truyền thống.
- Snapshot Backup
  - Người dùng có thể tạo snapshot thủ công.
  - Snapshot có thể được sao chép hoặc chia sẻ giữa các tài khoản AWS.
### Aurora Serverless
**Là phiên bản tự động co giãn của Aurora, không cần cấu hình cố định, chỉ hoạt động khi có truy vấn.**
- Cách hoạt động:
  - Tự động bật/tắt dựa trên nhu cầu sử dụng.
  - Chỉ trả phí theo thời gian thực tế sử dụng.
  - Phù hợp với ứng dụng có tải không ổn định hoặc lưu lượng thấp.
- Khi nào nên dùng Aurora Serverless?
  - Ứng dụng có lượng truy cập không ổn định.
  - Cần cắt giảm chi phí cho DB.
  - Không cần DB chạy 24/7.
 
## DynamoDB 
**Amazon DynamoDB là dịch vụ cơ sở dữ liệu NoSQL được AWS quản lý hoàn toàn, giúp lưu trữ và truy xuất dữ liệu với hiệu suất cao, tự động mở rộng, không cần quản lý hạ tầng.**
- Đặc điểm chính:
  - Không cần schema cố định như SQL.
  - Hỗ trợ cả key-value store và document store.
  - Scalability cao, mở rộng dễ dàng.
  - Tích hợp tốt với AWS Lambda, API Gateway, S3...
- Ưu điểm:
  - Tốc độ truy vấn millisecond latency.
  - Không cần cấu hình thủ công, AWS quản lý toàn bộ.
  - Tích hợp DynamoDB Streams, dễ dàng theo dõi thay đổi dữ liệu.
### High-Level DynamoDB
- DynamoDB bao gồm 3 thành phần chính:
  - Tables (Bảng): Lưu trữ dữ liệu theo dạng key-value.\
  - Items (Mục dữ liệu): Một hàng trong bảng.
  - Attributes (Thuộc tính): Các cột trong bảng, có thể là dữ liệu đơn giản hoặc phức tạp (JSON).
- Không cần thiết lập cluster.
- Tự động mở rộng với nhu cầu sử dụng.
- Hỗ trợ mạnh mẽ cho ứng dụng serverless.

### Read Consistency (Tính nhất quán khi đọc)
**DynamoDB cung cấp ba mức độ nhất quán khi đọc dữ liệu:**
- Eventually Consistent Reads (Nhất quán cuối cùng - Mặc định)
  - Dữ liệu có thể mất vài milliseconds để cập nhật trên tất cả nodes.
  - Phù hợp với ứng dụng không yêu cầu dữ liệu real-time.
- Strongly Consistent Reads (Nhất quán mạnh)
  - Đọc dữ liệu luôn trả về bản cập nhật mới nhất.
  - Có thể gây tăng độ trễ, giảm hiệu suất.
- Transactional Reads (Nhất quán giao dịch)
  - Dùng cho giao dịch nhiều bảng, đảm bảo tính nguyên tử (atomicity).

### DynamoDB Accelerator (DAX)
**DAX (DynamoDB Accelerator) là cache in-memory giúp tăng tốc truy vấn đọc lên đến 10 lần mà không cần thay đổi code ứng dụng.**
- Cách DAX hoạt động:
  - Lưu dữ liệu tạm thời trong bộ nhớ cache.
  - Khi có truy vấn, DAX kiểm tra cache trước khi gọi DynamoDB.
  - Giảm tải cho database chính, tăng hiệu suất.
- Khi nào nên dùng DAX?
  - Ứng dụng có lưu lượng truy vấn cao.
  - Cần tốc độ đọc nhanh, thời gian phản hồi dưới 1ms.
  - Muốn giảm tải DynamoDB, tiết kiệm chi phí.

### On-Demand Capacity (Dung lượng theo nhu cầu)
**Hai mô hình tính phí trong DynamoDB:**
- Provisioned Capacity (Mặc định)
  - Cấu hình trước số Read/Write Capacity Units (RCU/WCU).
  - Phù hợp với ứng dụng ổn định, tải dự đoán được.
- On-Demand Capacity (Theo nhu cầu)
  - Tự động mở rộng theo lưu lượng truy cập.
  - Chỉ trả phí cho số truy vấn thực tế, không cần dự báo trước.
  - Phù hợp với workload không ổn định hoặc mới khởi chạy.

### Bảo mật (Security)
**DynamoDB có các cơ chế bảo mật mạnh mẽ:**
- IAM Policies (Chính sách quyền hạn)
  - Kiểm soát truy cập DynamoDB bằng AWS Identity and Access Management (IAM).
- Encryption at Rest (Mã hóa dữ liệu lưu trữ)
  - Dữ liệu được mã hóa tự động bằng AWS KMS để bảo vệ thông tin nhạy cảm.
- Encryption in Transit (Mã hóa dữ liệu truyền tải)
  - Hỗ trợ TLS/SSL để bảo vệ dữ liệu khi truyền giữa client và DynamoDB.
- VPC Endpoints
  - Cho phép kết nối DynamoDB trong Amazon VPC mà không qua Internet công cộng.

### ACID và Transactions trong DynamoDB
**1. Sơ đồ ACID**  
ACID là viết tắt của **Atomicity, Consistency, Isolation, Durability**, bốn thuộc tính quan trọng đảm bảo tính tin cậy của giao dịch trong cơ sở dữ liệu.  

| Thuộc tính  | Ý nghĩa  | Ví dụ thực tế |
|-------------|---------|---------------|
| **Atomicity (Tính nguyên tử)** | Một giao dịch **hoặc hoàn thành toàn bộ hoặc không có gì xảy ra**. | Chuyển tiền giữa 2 tài khoản: Nếu trừ tiền tài khoản A nhưng chưa cộng vào tài khoản B, giao dịch sẽ bị hủy. |
| **Consistency (Tính nhất quán)** | Sau mỗi giao dịch, cơ sở dữ liệu phải duy trì một trạng thái hợp lệ theo các ràng buộc đã định. | Đảm bảo số dư tài khoản không thể âm sau giao dịch. |
| **Isolation (Tính cô lập)** | Các giao dịch thực hiện đồng thời không ảnh hưởng lẫn nhau. | Hai người dùng cùng cập nhật một tài khoản, nhưng chỉ có một giao dịch được chấp nhận đầu tiên. |
| **Durability (Tính bền vững)** | Dữ liệu được lưu vĩnh viễn ngay cả khi hệ thống gặp sự cố. | Sau khi xác nhận giao dịch, dữ liệu vẫn còn ngay cả khi mất điện. |

---

**2. ACID với DynamoDB**  
Mặc định, DynamoDB là **NoSQL**, được tối ưu cho hiệu suất cao và khả năng mở rộng, không hỗ trợ **ACID** như RDBMS.  

Tuy nhiên, từ năm 2018, DynamoDB hỗ trợ **Transactions**, giúp đảm bảo ACID trên nhiều bảng (multi-item transactions).  

**Cách DynamoDB hỗ trợ ACID:**  
- **Atomicity:** Nếu một giao dịch có nhiều thao tác, **hoặc tất cả thành công, hoặc tất cả bị hủy**.  
- **Consistency:** Chỉ lưu trạng thái hợp lệ, đảm bảo dữ liệu luôn chính xác.  
- **Isolation:** Không cho phép đọc dữ liệu chưa commit từ giao dịch khác.  
- **Durability:** Khi giao dịch hoàn tất, dữ liệu đã được ghi bền vững vào DynamoDB.  

**Công cụ hỗ trợ ACID trong DynamoDB:**  
- `TransactWriteItems`: Ghi dữ liệu giao dịch đảm bảo ACID.  
- `TransactGetItems`: Đọc dữ liệu đảm bảo tính nhất quán mạnh.  

**3. Tất cả (All) hoặc không có gì (Nothing)**  
DynamoDB thực thi nguyên tắc **"All or Nothing"** trong Transactions:  

=>  **Nếu một giao dịch có 5 thao tác nhưng 1 thao tác thất bại, thì toàn bộ giao dịch bị hủy bỏ.**  

**Ví dụ thực tế: Chuyển tiền giữa 2 tài khoản trong DynamoDB**  
```python
import boto3

dynamodb = boto3.client('dynamodb')

try:
    response = dynamodb.transact_write_items(
        TransactItems=[
            {
                'Update': {
                    'TableName': 'Accounts',
                    'Key': {'UserID': {'S': 'A'}},
                    'UpdateExpression': 'SET balance = balance - :amount',
                    'ExpressionAttributeValues': {':amount': {'N': '100'}},
                    'ConditionExpression': 'balance >= :amount'
                }
            },
            {
                'Update': {
                    'TableName': 'Accounts',
                    'Key': {'UserID': {'S': 'B'}},
                    'UpdateExpression': 'SET balance = balance + :amount',
                    'ExpressionAttributeValues': {':amount': {'N': '100'}}
                }
            }
        ]
    )
    print("Transaction succeeded")
except Exception as e:
    print("Transaction failed:", e)
```
=> Nếu tài khoản A không đủ tiền, toàn bộ giao dịch bị hủy. Không xảy ra trường hợp tài khoản A bị trừ tiền mà tài khoản B không nhận được.

**4. Các trường hợp sử dụng Transactions trong DynamoDB**
- Chuyển tiền giữa hai tài khoản
  - Đảm bảo cả hai thao tác (trừ tiền, cộng tiền) đều xảy ra hoặc bị hủy toàn bộ.
- Quản lý đặt hàng trong e-commerce
  - Trừ số lượng hàng trong kho.
  - Ghi nhận đơn hàng mới.
  - Nếu một bước thất bại → Hủy toàn bộ giao dịch.
- Xử lý đặt phòng khách sạn
  - Đảm bảo phòng chỉ được đặt một lần.
  - Nếu 2 người đặt cùng lúc, chỉ một giao dịch thành công.
- Cập nhật thông tin nhiều bảng cùng lúc
  - Ví dụ: Khi người dùng thay đổi địa chỉ, cần cập nhật cả bảng UserProfile và bảng Orders.
**5. Transactions trong DynamoDB**
- Ghi dữ liệu với Transactions
```
response = dynamodb.transact_write_items(
    TransactItems=[
        {
            'Put': {
                'TableName': 'Users',
                'Item': {'UserID': {'S': '123'}, 'Name': {'S': 'Alice'}}
            }
        },
        {
            'Put': {
                'TableName': 'Orders',
                'Item': {'OrderID': {'S': '789'}, 'UserID': {'S': '123'}}
            }
        }
    ]
)
```
- Đọc dữ liệu với Transactions
```
response = dynamodb.transact_get_items(
    TransactItems=[
        {
            'Get': {
                'TableName': 'Users',
                'Key': {'UserID': {'S': '123'}}
            }
        },
        {
            'Get': {
                'TableName': 'Orders',
                'Key': {'OrderID': {'S': '789'}}
            }
        }
    ]
)
```
### DynamoDB Backup
- On-Demand Backup and Restore
  - Full backup tại mọi thời điểm.
  - Không ảnh hưởng đến hiệu suất hoạt động hoặc khả năng sẵn sàng của bảng DynamoDB
  - Bản backup được thực hiện một cách nhất quán và hoàn thành trong vòng vài giây, sau đó được lưu trữ cho đến khi bị xoá.
  - Bản backup được lưu trữ trong cùng một AWS region với DynamoDB table gốc.
- Point-in-Time Recovery (PITR)
  - Bảo vệ chống lại ghi hoặc xoá dữ liệu vô tình.
  - khôi phục về bất kì thời điểm nào trong vòng 35 ngày.
  - thực hiện các bản backup gia tăng
  
### **Đặc điểm chính của DynamoDB Streams**
- **Ghi lại thay đổi dữ liệu**: Khi có một thao tác **INSERT, UPDATE hoặc DELETE** trên bảng, thông tin về thay đổi này sẽ được lưu trữ trong stream.
- **Giữ dữ liệu tối đa 24 giờ**: Các sự kiện chỉ tồn tại trong **24 giờ** trước khi bị xóa.
- **Thời gian thực**: Có thể kích hoạt Lambda để xử lý sự kiện ngay khi có thay đổi.
- **Tùy chọn dữ liệu ghi vào Stream**:
  - `KEYS_ONLY`: Chỉ chứa khóa chính (Primary Key).
  - `NEW_IMAGE`: Ghi lại toàn bộ bản ghi mới sau khi thay đổi.
  - `OLD_IMAGE`: Ghi lại toàn bộ bản ghi cũ trước khi thay đổi.
  - `NEW_AND_OLD_IMAGES`: Ghi lại cả bản ghi trước và sau khi thay đổi.
 
### **Đặc điểm chính của Global Tables**
DynamoDB Global Tables là một tính năng giúp **sao chép dữ liệu giữa nhiều AWS Regions** theo cách tự động và có độ trễ thấp. Điều này giúp ứng dụng có khả năng **đa vùng (multi-region), tăng tính sẵn sàng (high availability) và khả năng chịu lỗi (fault tolerance)**.
- **Đồng bộ dữ liệu đa vùng tự động**: Khi có thay đổi trong một region, dữ liệu sẽ được cập nhật sang các region khác.
- **Hoạt động theo mô hình Active-Active**: Ứng dụng có thể đọc/ghi dữ liệu từ bất kỳ region nào.
- **Hỗ trợ tính nhất quán cuối cùng (Eventually Consistent)**.
- **Tích hợp DynamoDB Streams** để theo dõi thay đổi trên bảng.
- **Giảm độ trễ truy cập**: Người dùng có thể truy cập dữ liệu từ region gần nhất.

##  MongoDB and Amazon DocumentDB 
MongoDB là một hệ quản trị cơ sở dữ liệu **NoSQL dạng document** phổ biến, được thiết kế để lưu trữ dữ liệu dưới dạng **JSON/BSON** thay vì bảng như trong cơ sở dữ liệu quan hệ (RDBMS).

### Đặc điểm chính của MongoDB:
- **Mô hình document (Tài liệu)**: Dữ liệu được lưu trữ trong các tài liệu JSON/BSON giúp linh hoạt hơn so với bảng truyền thống.
- **Không có lược đồ cố định (Schema-less)**: Dữ liệu có thể có cấu trúc khác nhau giữa các tài liệu trong cùng một collection.
- **Hỗ trợ mở rộng theo chiều ngang (Horizontal Scaling)** với **Sharding**.
- **Hiệu suất cao** với khả năng truy vấn linh hoạt và lưu trữ dữ liệu có cấu trúc phức tạp.
- **Replication & High Availability**: Hỗ trợ mô hình Replica Set để đảm bảo tính sẵn sàng.
- **Hỗ trợ Indexing**: Giúp truy vấn dữ liệu nhanh hơn.

---

### Ví dụ về MongoDB
Giả sử bạn có một hệ thống quản lý thông tin người dùng, MongoDB lưu dữ liệu dưới dạng JSON như sau:

```json
{
  "_id": "001",
  "name": "Nguyễn Văn A",
  "email": "nguyenvana@example.com",
  "phone": "0987654321",
  "address": {
    "street": "123 Đường ABC",
    "city": "Hà Nội"
  },
  "orders": [
    {
      "order_id": "1001",
      "total": 500000,
      "status": "Completed"
    },
    {
      "order_id": "1002",
      "total": 300000,
      "status": "Pending"
    }
  ]
}
```
### Amazon DocumentDB 
**Amazon DocumentDB là một dịch vụ cơ sở dữ liệu NoSQL do AWS cung cấp, được tối ưu hóa để chạy ứng dụng tương thích với MongoDB nhưng với hiệu suất cao hơn, bảo mật tốt hơn và khả năng mở rộng linh hoạt hơn.**
- Đặc điểm chính của Amazon DocumentDB:
  - Tương thích với MongoDB: Hỗ trợ hầu hết các API của MongoDB, giúp dễ dàng di chuyển ứng dụng từ MongoDB sang AWS.
  - Managed Service: AWS quản lý hoàn toàn, không cần lo về cài đặt, cập nhật, hoặc bảo trì.
  - Tăng tốc hiệu suất: Amazon DocumentDB được tối ưu để xử lý khối lượng truy vấn lớn nhanh hơn so với MongoDB truyền thống.
  - Tự động sao lưu và phục hồi (Automatic Backup & Restore).
  - Tích hợp AWS IAM, VPC, Encryption giúp tăng cường bảo mật.
- Lý do chọn DocumentDB thay vì MongoDB tự quản lý:
  - Không cần quản trị hạ tầng: AWS tự động quản lý phần backend, giảm tải công việc DevOps.
  - Hiệu suất cao hơn: DocumentDB được tối ưu hóa để xử lý khối lượng công việc lớn nhanh hơn.
  - Mở rộng linh hoạt: Có thể tăng hoặc giảm dung lượng theo nhu cầu mà không cần downtime.
  - Tính sẵn sàng cao: Hỗ trợ Multi-AZ replication, tự động sao lưu, phục hồi nhanh chóng.
  - Tích hợp với AWS Services: Dễ dàng kết nối với Lambda, Kinesis, S3, Glue...
 
## **Apache Cassandra và Keyspaces**
### Cassandra 
Apache Cassandra là một **cơ sở dữ liệu phân tán NoSQL** được thiết kế để quản lý **lượng dữ liệu lớn trên nhiều node** mà vẫn đảm bảo tính sẵn sàng và không có điểm thất bại đơn lẻ.
- Đặc điểm chính của Cassandra:
  - **Phân tán hoàn toàn (Fully Distributed):** Không có node trung tâm, tất cả các node đều ngang hàng.
  - **Khả năng mở rộng theo chiều ngang (Horizontal Scalability):** Có thể mở rộng bằng cách thêm nhiều node mà không ảnh hưởng đến hiệu suất.
  - **Tính sẵn sàng cao (High Availability):** Hỗ trợ replication trên nhiều data center, đảm bảo dữ liệu luôn có thể truy cập.
  - **Không có lược đồ cố định (Schema-Free):** Dữ liệu có thể thay đổi linh hoạt, phù hợp với mô hình NoSQL.
  - **Hiệu suất cao:** Tối ưu hóa cho **write-heavy workload** và hỗ trợ **partitioning bằng hash** để truy vấn nhanh.

### AWS Keyspaces
**AWS Keyspaces là dịch vụ cơ sở dữ liệu NoSQL được quản lý hoàn toàn, tương thích với Apache Cassandra, giúp bạn lưu trữ và truy vấn dữ liệu mà không cần quản lý cơ sở hạ tầng.
AWS Keyspaces cho phép bạn chạy các workload Cassandra mà không cần tự triển khai hoặc quản lý cluster Cassandra, giúp tối ưu về hiệu suất, bảo mật và khả năng mở rộng.**
- Không cần quản lý cơ sở hạ tầng: AWS tự động mở rộng, sao lưu và bảo trì hệ thống.
- Tương thích với Apache Cassandra: Hỗ trợ CQL (Cassandra Query Language), giúp bạn sử dụng lại code hiện có.
- Khả năng mở rộng theo chiều ngang: Dịch vụ có thể mở rộng tự động để đáp ứng nhu cầu.
- Hiệu suất cao: Độ trễ thấp, có thể xử lý hàng triệu truy vấn mỗi giây.
- Tích hợp với AWS IAM: Hỗ trợ bảo mật cấp độ doanh nghiệp bằng cách kiểm soát truy cập với IAM Policies.

## Graph Database và Amazon Neptune
### Graph Database
**Graph Database (Cơ sở dữ liệu đồ thị) là cơ sở dữ liệu được thiết kế để lưu trữ và truy vấn dữ liệu dưới dạng đồ thị. Nó khác với cơ sở dữ liệu quan hệ (RDBMS) ở chỗ dữ liệu được tổ chức theo các nút (nodes) và quan hệ (edges) thay vì bảng và hàng.**
- Đặc điểm của Graph Database
  - Nút (Node): Đại diện cho thực thể (ví dụ: người dùng, sản phẩm, địa điểm).
  - Cạnh (Edge): Đại diện cho mối quan hệ giữa các thực thể (ví dụ: "Alice là bạn của Bob").
  - Thuộc tính (Properties): Lưu trữ thông tin về nút hoặc cạnh (ví dụ: tên, tuổi, địa chỉ).
  - Truy vấn theo quan hệ nhanh: Graph Database tối ưu hóa cho các truy vấn quan hệ phức tạp, giúp phân tích mạng xã hội, đề xuất sản phẩm, phát hiện gian lận, v.v.
### Amazon Neptune
**Amazon Neptune là dịch vụ cơ sở dữ liệu đồ thị được quản lý hoàn toàn bởi AWS**
**Các trường hợp sử dụng với Neptune**
- Xây dựng Kết nối giữa các Danh tính (Identities)
  - Dễ dàng xây dựng các biểu đồ danh tính để giải quyết danh tính, VD như biểu đồ xã hội, và tăng tốc cập nhật cho việc nhằm mục tiêu quảng cáo, cá nhân hóa, và phân tích.
- Phát hiện Các Mẫu (Patterns) Gian lận (Fraud)
  - Xây dựng các truy vấn biểu đồ để phát hiện các mẫu gian lận danh tính gần như gần với thời gian thực trong các giao dịch tài chính và mua hàng.
- Xây dựng Ứng dụng biểu đồ Kiến thức
  - Thêm dữ liệu theo chủ đề vào các danh mục sản phẩm, mô hình hóa thông tin tổng quát và giúp users nhanh chóng chuyển hướng các datasets có kết nối cao.
- Biểu đồ Bảo mật để Cải thiện IT Security
  - Chủ động phát hiện và điều tra hạ tầng IT sử dụng phương pháp bảo mật nhiều lớp. Có một cái nhìn tổng quan về toàn bộ hạ tầng để lên kế hoạch, dự đoán, và giảm thiểu rủi ro.

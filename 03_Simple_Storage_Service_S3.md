# Amazon S3 (Simple Storage Service)
**Amazon S3 (Simple Storage Service) là dịch vụ lưu trữ đối tượng (object storage) trên đám mây của AWS, cho phép lưu trữ và truy xuất dữ liệu ở mọi quy mô, từ dữ liệu nhỏ vài KB đến dữ liệu lớn hàng TB.**
## Đặc điểm chính
- Lưu trữ đối tượng (Object Storage)
  - Dữ liệu được lưu dưới dạng các object (tệp) kèm theo metadata (siêu dữ liệu mô tả tệp).
  - Không phân cấp thư mục truyền thống như hệ thống file, nhưng AWS S3 mô phỏng thư mục bằng cách dùng prefix (tiền tố) trong tên tệp.
- Buckets
  - Mọi object đều được lưu trữ trong bucket.
  - Bucket phải có tên duy nhất trên toàn cầu.
  - Có thể thiết lập region để đặt bucket gần người dùng hoặc ứng dụng nhằm giảm độ trễ.
- Khả năng mở rộng & độ bền
  - S3 được thiết kế với độ bền 99.999999999% (11 số 9) và tính sẵn sàng 99.99%.
  - Tự động phân tán dữ liệu trong nhiều AZ (Availability Zones) để giảm rủi ro mất dữ liệu.
## Các lớp lưu trữ (Storage Classes)
- S3 Standard
  - Phù hợp cho dữ liệu truy cập thường xuyên, đòi hỏi độ trễ thấp.
  - Độ bền 11 số 9, tính sẵn sàng 99.99%.
- S3 Standard-Infrequent Access (S3 Standard-IA)
  - Dành cho dữ liệu ít được truy cập, nhưng vẫn cần truy xuất nhanh khi cần.
  - Chi phí lưu trữ thấp hơn S3 Standard, nhưng phí lấy dữ liệu cao hơn.
- S3 One Zone-Infrequent Access
  - Tương tự S3 Standard-IA nhưng chỉ lưu trữ trong một AZ duy nhất.
  - Nguy cơ mất dữ liệu cao hơn, giá rẻ hơn.
- S3 Glacier Instant Retrieval
  - Lưu trữ dữ liệu cần truy cập không thường xuyên, nhưng khi truy cập phải có ngay (milliseconds).
  - Chi phí rất thấp, phù hợp lưu trữ dài hạn.
- S3 Glacier Flexible Retrieval (Trước đây là S3 Glacier)
  - Dữ liệu truy cập rất hiếm khi cần, thời gian truy xuất có thể lên tới vài phút (hoặc lâu hơn).
  - Giá rẻ hơn Glacier Instant Retrieval.
- S3 Glacier Deep Archive
  - Lớp lưu trữ rẻ nhất, dành cho dữ liệu gần như không truy cập.
  - Thời gian truy xuất từ 12 - 48 giờ.
- S3 Intelligent-Tiering
  - Tự động chuyển dữ liệu qua lại giữa các lớp lưu trữ (Standard, IA, Archive) dựa trên tần suất truy cập, giúp tối ưu chi phí mà không cần quản lý thủ công.
## Các tính năng nổi bật
- Versioning (Phiên bản)
  - Cho phép lưu nhiều phiên bản của cùng một object.
  - Tránh mất mát dữ liệu do ghi đè, xóa nhầm.
- Lifecycle Management
  - Định nghĩa chu kỳ vòng đời của dữ liệu.
  - Tự động chuyển object sang lớp lưu trữ rẻ hơn (IA, Glacier, v.v.) sau một thời gian.
  - Tự động xóa object khi hết vòng đời.
- Cross-Region Replication (CRR)
  - Tự động sao chép object từ bucket nguồn sang bucket đích ở Region khác.
  - Tăng khả năng phục hồi thảm họa, đảm bảo dữ liệu luôn có sẵn ở nhiều khu vực địa lý.
- Encryption (Mã hóa)
  - Hỗ trợ Server-Side Encryption (SSE) và Client-Side Encryption.
  - AWS KMS (Key Management Service) để quản lý khóa.
- Access Control & Security
  - IAM Policies: Quản lý quyền truy cập ở cấp tài khoản.
  - Bucket Policies: Quy định quyền cho bucket hoặc object.
- ACL (Access Control List): Quản lý quyền truy cập ở cấp đối tượng.
  - Block Public Access: Tính năng chặn toàn bộ truy cập công khai (public) cho bucket.
- Event Notifications
  - Khi có sự kiện như tải lên (PUT), xóa (DELETE), S3 có thể gửi thông báo đến Amazon SNS, SQS hoặc AWS Lambda.
  - Hữu ích trong việc xây dựng pipeline xử lý dữ liệu tự động.
- Hosting tĩnh (Static Website Hosting)
  - Dùng S3 để lưu trữ và phục vụ website tĩnh (HTML, CSS, JS).
  - Kết hợp với Amazon CloudFront để tăng tốc phân phối nội dung.
- Query in place (S3 Select / Athena)
  - S3 Select: Truy vấn dữ liệu có cấu trúc (CSV, JSON, Parquet) trực tiếp trong S3, chỉ trả về kết quả cần thiết.
  - Amazon Athena: Dịch vụ truy vấn serverless trên dữ liệu S3 bằng ngôn ngữ SQL.
## Mã hoá
- Mã hóa trong quá trình truyền tải (Encryption in Transit):
  - Sử dụng SSL/TLS và HTTPS để bảo vệ dữ liệu khi truyền qua mạng.
- Mã hóa tại trạng thái nghỉ (Encryption at Rest) - Server-Side Encryption (SSE):
  - SSE-S3: Sử dụng AES-256-bit.
  - SSE-KMS: Sử dụng AWS Key Management Service để quản lý khóa mã hóa.
  - SSE-C: Người dùng tự quản lý khóa mã hóa.
- Mã hóa phía khách hàng (Client-Side Encryption):
  - Người dùng tự mã hóa dữ liệu trước khi tải lên S3.
- Cưỡng chế mã hóa bằng Bucket Policy (Enforcing Encryption with Bucket Policy):
  - Có thể thiết lập Bucket Policy để từ chối các yêu cầu tải lên nếu thiếu x-amz-server-side-encryption trong request header.
## Tối ưu hóa hiệu suất Amazon S3
### S3 Prefix
***AWS S3 xử lý dữ liệu theo prefix-based sharding (phân tán theo tiền tố). Điều này có nghĩa là nếu quá nhiều đối tượng có cùng tiền tố, hiệu suất có thể bị ảnh hưởng do xung đột truy cập (hotspot).***
- **Ví dụ**
```
mybucketname/folder1/subfolder1/myfile.jpg > /folder1/subfolder1
mybucketname/folder2/subfolder1/myfile.jpg > /folder2/subfolder1
mybucketname/folder3/myfile.jpg > /folder3
mybucketname/folder4/subfolder4/myfile.jpg > /folder4/subfolder4
```
=> nếu sử dụng cả 4 prefix thì hiệu suất sẽ x4
### Các giới hạn với KMS
- Tốc độ yêu cầu mã hóa/giải mã:
  - 5.500 yêu cầu/giây đối với khóa KMS SYMMETRIC.
  - 100 yêu cầu/giây đối với khóa KMS ASYMMETRIC.

=> Nếu số lượng yêu cầu vượt quá giới hạn, S3 sẽ bị chậm hoặc thất bại khi tải lên/xuống dữ liệu.
- Chi phí tăng cao do KMS API calls
  - Mỗi lần tải PUT (ghi) hoặc GET (đọc) với SSE-KMS, S3 gọi API đến KMS để mã hóa/giải mã dữ liệu.
  - Nếu có hàng triệu file, chi phí KMS có thể tăng nhanh
- Giới hạn về kích thước dữ liệu
  - KMS không trực tiếp mã hóa dữ liệu lớn
  - AWS KMS chỉ mã hóa một data key (~256-bit), còn dữ liệu thực tế được mã hóa bằng data key này.
  - Amazon S3 xử lý mã hóa theo khối lớn, nhưng vẫn cần gọi KMS để lấy key.
###  Sử dụng Multi-Part Upload
- Khi tải lên tệp lớn (>100MB), chia nhỏ dữ liệu thành nhiều phần để tải lên song song.
- Cải thiện hiệu suất và đảm bảo khả năng tiếp tục tải nếu bị gián đoạn.
![image](https://github.com/user-attachments/assets/7be5bc8a-c7c3-424b-b173-cf1ff2988cd2)

### Sử dụng Byte-Range Fetch
- Khi tải xuống, yêu cầu chỉ một phần dữ liệu (byte range) để giảm tải cho S3.
- Tăng tốc độ tải về khi làm việc với tệp lớn.
![image](https://github.com/user-attachments/assets/cafb4fbb-8c3f-4f27-ba9d-5e868c34cd5a)

## Amazon S3 Replication
**Amazon S3 Replication là tính năng sao chép dữ liệu tự động, không đồng bộ giữa các bucket S3, giúp bảo vệ dữ liệu, tuân thủ quy định và tối ưu hiệu suất truy cập.**
### Các loại S3 Replication
- Cross-Region Replication (CRR) Sao chép dữ liệu giữa các vùng (Region) khác nhau
  - Cần dự phòng dữ liệu (disaster recovery)
  - Giảm độ trễ truy cập cho người dùng ở nhiều khu vực
  - Tuân thủ quy định yêu cầu lưu dữ liệu ở nhiều Region
- Same-Region Replication (SRR) Sao chép dữ liệu trong cùng một Region
  - Lưu trữ dữ liệu trong nhiều bucket để quản lý riêng biệt
  - Hỗ trợ compliance (ví dụ: tách dữ liệu gốc & backup)
  - Chuyển dữ liệu giữa tài khoản AWS khác nhau
### Cách hoạt động của S3 Replication
- Khi một object mới được tải lên bucket nguồn, nó sẽ được sao chép tự động sang bucket đích.
- Replication chỉ áp dụng với object mới, không sao chép object đã có trước khi bật replication.
- Có thể dùng Replication Time Control (RTC) để cam kết sao chép trong vòng 15 phút.

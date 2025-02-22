# CloudWatch
**AWS CloudWatch là một dịch vụ giám sát và quan sát trên AWS, giúp thu thập và phân tích dữ liệu từ tài nguyên, ứng dụng và dịch vụ để đảm bảo hiệu suất và bảo mật.**
## CloudWatch Metrics
- Thu thập số liệu tự động: Các dịch vụ AWS như EC2, RDS, S3, Lambda, và nhiều dịch vụ khác đều tự động gửi số liệu về CloudWatch. Ví dụ, EC2 cung cấp số liệu về CPU, bộ nhớ (nếu tích hợp thêm), băng thông mạng, đĩa I/O,…
- Custom Metrics: Ngoài các số liệu mặc định, có thể gửi các số liệu tùy chỉnh từ ứng dụng hoặc hệ thống của mình. Điều này cho phép theo dõi các chỉ số chuyên biệt như số lượng giao dịch, thời gian phản hồi API,…
- Độ phân giải và Retention:
  - Các metric tiêu chuẩn thường có độ phân giải 1 phút.
  - Với High-Resolution Metrics, có thể thu thập số liệu với độ phân giải dưới 1 phút (ví dụ: 1 giây) để theo dõi các thay đổi rất nhanh.
  - Dữ liệu metric được lưu trữ với thời gian khác nhau (thường là vài tháng) tùy theo loại số liệu, giúp  có thể xem lại lịch sử hoạt động.
## CloudWatch Logs
- Thu thập và Lưu trữ Log: CloudWatch Logs cho phép thu thập log từ nhiều nguồn như EC2, Lambda, ECS, API Gateway, và ứng dụng do  tự phát triển.  có thể tạo Log Groups (nhóm log) và Log Streams (luồng log) để tổ chức dữ liệu.
- Log Insights: Đây là công cụ truy vấn mạnh mẽ, giúp  tìm kiếm, phân tích và trực quan hóa log thông qua ngôn ngữ truy vấn riêng. Có thể lọc ra các thông tin quan trọng hoặc theo dõi các sự kiện bất thường.
- Retention Policy: Có thể cấu hình khoảng thời gian lưu trữ log tùy theo nhu cầu, từ vài giờ đến vài tháng hoặc vô thời hạn, giúp kiểm soát chi phí lưu trữ và tuân thủ các yêu cầu bảo mật, lưu trữ dữ liệu.
- Phân tích thời gian thực: CloudWatch Logs có thể được cấu hình để chuyển tiếp log đến các dịch vụ khác như Amazon Elasticsearch Service (OpenSearch Service) để phân tích sâu hơn hoặc thực hiện các hành động tự động.
## CloudWatch Alarms
- Cài đặt Ngưỡng và Cảnh báo:
 có thể đặt các cảnh báo dựa trên các metric (ví dụ: CPU Utilization vượt quá 80% trong 5 phút) và thiết lập ngưỡng để nhận thông báo khi có bất thường.

- Hành động Tự động:
Khi alarm được kích hoạt,  có thể cấu hình để gửi thông báo qua SNS, kích hoạt AWS Lambda, hoặc thực hiện các tác vụ như Auto Scaling nhằm tự động mở rộng hay thu hẹp tài nguyên.

- Trạng thái Alarm:
Alarm có thể có các trạng thái như ALARM (khi ngưỡng vượt qua), OK (bình thường), và INSUFFICIENT DATA (thiếu dữ liệu để đánh giá), giúp  nắm bắt tình trạng hệ thống một cách chính xác.

- Composite Alarms:
Cho phép kết hợp nhiều alarm lại với nhau để tạo ra một cảnh báo tổng hợp, giúp giảm thiểu số lượng cảnh báo giả và tập trung vào các vấn đề quan trọng.
## CloudWatch Events / EventBridge
- Quản lý Sự kiện:
Dịch vụ này (hiện đã phát triển thêm với tên gọi EventBridge) theo dõi và xử lý các sự kiện xảy ra trong hệ thống. Mỗi khi có thay đổi trạng thái từ tài nguyên AWS (như EC2 instance khởi động hay dừng) hoặc sự kiện từ ứng dụng, CloudWatch có thể nhận diện và xử lý.

- Quy tắc và Mẫu sự kiện (Event Patterns):
 có thể định nghĩa các quy tắc để lọc và chuyển hướng sự kiện đến các mục tiêu như AWS Lambda, SNS, SQS, Step Functions, v.v. Điều này giúp tự động hóa quy trình phản ứng theo sự kiện.

- Tích hợp với SaaS và các nguồn bên ngoài:
EventBridge không chỉ nhận sự kiện từ AWS mà còn hỗ trợ tích hợp với các dịch vụ phần mềm bên thứ ba, mở rộng khả năng ứng dụng trong nhiều kịch bản tự động hóa.
## CloudWatch Synthetics
- Canaries (Kịch bản kiểm tra tự động):
Cho phép  tạo các kịch bản tự động (canaries) để mô phỏng hành vi của người dùng truy cập website, API hoặc ứng dụng. Điều này giúp phát hiện sớm các lỗi hiệu suất hoặc downtime.

- Kiểm tra Định kỳ:
 có thể cài đặt lịch trình chạy canaries để kiểm tra liên tục độ sẵn sàng và hiệu suất của các endpoint quan trọng, đảm bảo rằng hệ thống hoạt động ổn định trước khi người dùng thực sự truy cập.

- Báo cáo Chi tiết:
Kết quả của các lần chạy canaries sẽ được lưu lại và trực quan hóa trên CloudWatch, giúp  phân tích nguyên nhân của sự cố hoặc giảm thiểu rủi ro cho người dùng.
## CloudWatch ServiceLens
- Tích hợp Dữ liệu Đa chiều:
ServiceLens mang đến một cái nhìn tổng hợp về các số liệu, log, trace và alarm, giúp  theo dõi toàn bộ quá trình xử lý của ứng dụng.

- Kết nối với AWS X-Ray:
Dịch vụ tích hợp trực tiếp với AWS X-Ray để cung cấp tính năng trace (truy vết) cho các ứng dụng phân tán, giúp xác định nhanh các điểm nghẽn hoặc lỗi trong hệ thống.

- Giao diện Trực quan:
Với ServiceLens,  có thể xem được mối quan hệ giữa các thành phần của hệ thống, dễ dàng phát hiện ra các lỗi thời gian thực và hành động khắc phục nhanh chóng.
## CloudWatch Dashboards
- Tùy chỉnh Giao diện:
Cho phép  xây dựng các bảng điều khiển trực quan với các widget, biểu đồ và số liệu từ nhiều nguồn. Điều này giúp  theo dõi nhanh tình trạng hệ thống từ một giao diện tập trung.

- Tích hợp Nhiều Khu vực và Tài khoản:
Dashboards có thể hiển thị dữ liệu từ nhiều vùng (region) khác nhau và cả từ nhiều tài khoản AWS, hỗ trợ quản lý hệ thống phức tạp trong tổ chức.

- Chia sẻ và Hợp tác:
 có thể chia sẻ dashboard với các đồng nghiệp hoặc đội ngũ vận hành, đảm bảo mọi người đều có cùng một cái nhìn tổng quát về hiệu suất hệ thống.

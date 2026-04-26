# Services Overview — Giải Thích Từng Dịch Vụ AWS

File này giải thích ngắn gọn từng dịch vụ AWS xuất hiện trong SAA-C03 bằng ngôn ngữ đơn giản. Mỗi mục trả lời: **dịch vụ này làm gì, dùng khi nào**.

---

## Security & Identity

### IAM
**AWS Identity and Access Management** — Hệ thống kiểm soát "ai được làm gì" trên AWS. Bạn tạo user, group, role và gán permission cho từng đối tượng. Mọi request tới AWS đều đi qua IAM để kiểm tra quyền.

### IAM Identity Center
Cổng đăng nhập tập trung cho nhân viên (workforce) cần truy cập nhiều AWS account cùng lúc. Thay vì tạo IAM user riêng trong từng account, bạn quản lý ở một chỗ và cấp quyền theo permission set. Tích hợp được với Active Directory, Okta, Azure AD.

### Organizations
Dịch vụ gom nhiều AWS account vào một tổ chức để quản lý tập trung. Dùng khi công ty có nhiều team/product mỗi cái một account riêng. Hỗ trợ consolidated billing (một hóa đơn cho tất cả) và SCP để đặt guardrail.

### Control Tower
"Bộ khởi động" để thiết lập môi trường nhiều account chuẩn hóa (gọi là landing zone). AWS tự động tạo sẵn cấu trúc account, SCP cơ bản, logging, và audit trail theo best practice. Dùng khi bắt đầu xây dựng môi trường enterprise từ đầu.

### SCP
**Service Control Policy** — Quy tắc giới hạn quyền tối đa áp dụng cho toàn bộ account hoặc nhóm account (OU) trong Organizations. Ví dụ: SCP chặn mọi người dùng dịch vụ ngoài Region ap-southeast-1. SCP không cấp quyền, chỉ thu hẹp quyền. Xem thêm [Từ Điển → SCP](glossary.md#scp).

---

## Networking

### VPC
**Virtual Private Cloud** — Mạng riêng ảo của bạn trên AWS. Giống như bạn được cấp một khu đất riêng trong data center, bạn tự chia ô, đặt tường, làm đường. Mọi resource như EC2, RDS đều chạy trong VPC. Xem thêm [Từ Điển → VPC](glossary.md#vpc).

### Subnet
Ô con bên trong VPC, gắn với một AZ cụ thể. **Public subnet** có đường ra internet trực tiếp (qua Internet Gateway). **Private subnet** không có đường ra internet trực tiếp — phù hợp để đặt database và app server nội bộ.

### Route Table
Bảng chỉ đường cho traffic trong VPC. Mỗi subnet gắn với một route table. Rule trong bảng xác định traffic đi đến đích nào (Internet Gateway, NAT Gateway, VPC Endpoint...). Rule cụ thể hơn luôn thắng rule rộng hơn.

### Security Group
Tường lửa ảo ở cấp độ từng server/container. Chỉ có rule cho phép (allow), không có rule chặn (deny). **Stateful**: khi cho phép traffic vào, traffic trả về tự động được phép mà không cần rule riêng. Xem thêm [Từ Điển → Security Group](glossary.md#security-group).

### Network ACL
Tường lửa ở cấp độ subnet. Khác Security Group ở chỗ: **stateless** (phải tạo rule cho cả chiều vào lẫn chiều ra), có thể chặn (deny) một IP cụ thể. Áp dụng cho toàn bộ subnet, không phải từng server.

### NAT Gateway
Cổng giúp server trong private subnet gửi request ra internet (ví dụ: download bản vá bảo mật) mà không bị lộ địa chỉ IP nội bộ ra ngoài. Chỉ cho traffic đi ra, không cho traffic từ internet vào. Xem thêm [Từ Điển → NAT](glossary.md#nat).

### VPC Endpoints
Đường hầm riêng từ VPC tới dịch vụ AWS mà không cần đi qua internet công cộng. Tăng bảo mật và giảm phí dữ liệu. Có hai loại: **Gateway Endpoint** (S3, DynamoDB — miễn phí theo giờ) và **Interface Endpoint** (hầu hết dịch vụ khác — tính phí theo giờ).

### ELB — ALB / NLB / Gateway Load Balancer
**Elastic Load Balancing** — Bộ phân phối traffic đến nhiều server để không server nào bị quá tải và hệ thống vẫn chạy khi một server hỏng.
- **ALB** (Application): phân phối theo URL path/domain, dùng cho web app.
- **NLB** (Network): phân phối TCP/UDP cực nhanh, dùng cho app cần độ trễ thấp hoặc IP tĩnh.
- **Gateway LB**: chèn thiết bị bảo mật (firewall, IDS) vào giữa traffic một cách trong suốt.

### Route 53
Dịch vụ DNS của AWS — dịch tên miền (`myapp.com`) sang địa chỉ IP của server. Ngoài DNS, còn hỗ trợ health check và routing thông minh: tự chuyển user đến server gần nhất, server còn sống, hoặc theo tỷ lệ A/B test.

### CloudFront
Mạng phân phối nội dung (CDN) toàn cầu của AWS. Cache website, hình ảnh, video ở hàng trăm edge location gần người dùng, giảm thời gian tải trang. Tích hợp WAF để chặn tấn công tại edge.

### Global Accelerator
Tăng tốc kết nối mạng bằng cách đưa traffic của user vào backbone mạng riêng của AWS ngay từ điểm gần nhất, thay vì đi qua internet công cộng nhiều bước. Khác CloudFront ở chỗ không cache — tốt cho TCP/UDP app cần giảm latency và failover nhanh.

### Direct Connect
Đường dây vật lý riêng giữa data center của bạn và AWS, không đi qua internet công cộng. Băng thông ổn định, độ trễ thấp hơn VPN. **Không mã hóa mặc định** — cần kết hợp VPN nếu cần mã hóa.

### Site-to-Site VPN
Đường hầm mã hóa qua internet công cộng giữa on-premises và VPC. Triển khai nhanh hơn Direct Connect nhưng bandwidth và latency phụ thuộc vào đường internet.

### Transit Gateway
Hub trung tâm kết nối nhiều VPC, VPN, và Direct Connect với nhau. Thay vì phải kết nối từng cặp VPC (N×(N-1)/2 kết nối), Transit Gateway cho phép mọi thứ kết nối qua một điểm duy nhất.

### PrivateLink
Công nghệ expose một dịch vụ (của bạn hoặc của partner) cho VPC khác truy cập một chiều mà không cần peering toàn mạng. Consumer chỉ thấy đúng endpoint đó, không thấy phần còn lại của mạng provider.

---

## Compute

### EC2
**Elastic Compute Cloud** — Máy chủ ảo thuê theo giờ trên AWS. Bạn chọn hệ điều hành, cấu hình CPU/RAM, ổ đĩa và mạng. Mức kiểm soát cao nhất trong các lựa chọn compute của AWS.

### Auto Scaling
Cơ chế tự động tăng/giảm số lượng EC2 instance theo nhu cầu thực tế. Khi traffic tăng thì thêm máy; khi traffic giảm thì bớt máy. Giúp hệ thống luôn đủ năng lực và không lãng phí chi phí.

### Launch Template
Bản mẫu cấu hình EC2: AMI (hệ điều hành), instance type, security group, key pair, user data script... Khi Auto Scaling cần tạo máy mới, nó dùng Launch Template này làm khuôn.

### Placement Group
Cách sắp xếp vật lý EC2 instances trong data center:
- **Cluster**: nhóm các máy lại gần nhau để có latency cực thấp (HPC, ML training).
- **Spread**: mỗi máy trên phần cứng khác nhau để giảm rủi ro hỏng đồng loạt.
- **Partition**: chia nhóm partition để hệ thống phân tán như Kafka, Cassandra có fault isolation.

### Spot Instances
Mua EC2 dư thừa của AWS với giá rẻ hơn tới 90%, nhưng AWS có thể thu hồi khi cần tài nguyên. Phù hợp cho batch job, rendering, ML training — những tác vụ chịu được interrupt.

### Reserved Instances
Cam kết thuê EC2 (hoặc RDS, ElastiCache) trong 1 hoặc 3 năm để nhận giảm giá sâu (tới 72% so với On-Demand). Phù hợp cho workload chạy liên tục, ổn định.

### Savings Plans
Cam kết chi tiêu một số tiền nhất định mỗi giờ trong 1-3 năm để nhận giảm giá. Linh hoạt hơn Reserved Instances vì không cần chỉ định instance type/region. Áp dụng cho EC2, Fargate, Lambda.

### Lambda
Chạy code mà không cần quản lý server. Bạn upload code, AWS lo hạ tầng, tự động scale từ 0 đến hàng nghìn request song song. Tính tiền theo thời gian chạy thực tế (100ms). Timeout tối đa 15 phút.

### Fargate
Engine chạy container mà không cần quản lý EC2. Bạn chỉ cần định nghĩa container (CPU, RAM, image), Fargate lo phần còn lại. Dùng với ECS hoặc EKS.

### ECS
**Elastic Container Service** — Nền tảng điều phối container của AWS. Quản lý việc chạy, dừng, restart container, phân phối đều tải, và đảm bảo số lượng container theo yêu cầu. Chạy trên EC2 hoặc Fargate.

### EKS
**Elastic Kubernetes Service** — Kubernetes được AWS quản lý phần control plane. Dùng khi tổ chức đã dùng Kubernetes và muốn chạy trên AWS mà không tự vận hành master node.

### ECR
**Elastic Container Registry** — Kho lưu trữ Docker image riêng tư trên AWS. Tích hợp với ECS/EKS để pull image khi deploy.

---

## Storage

### S3
**Simple Storage Service** — Kho lưu trữ object (file) với dung lượng không giới hạn. Lưu mọi thứ: hình ảnh, video, log, backup, dataset. Độ bền 11 số 9 (gần như không bao giờ mất dữ liệu). Không phải ổ cứng truyền thống — không mount, không chỉnh sửa trực tiếp từng byte.

### S3 Glacier
Lớp lưu trữ siêu rẻ trong S3 dành cho dữ liệu ít khi cần đọc lại: archive, backup dài hạn, dữ liệu compliance. Đổi lại: lấy dữ liệu ra mất từ vài phút đến vài giờ (tùy loại Glacier). Rẻ hơn S3 Standard nhiều lần.

### EBS
**Elastic Block Store** — Ổ cứng ảo gắn vào EC2. Mỗi volume chỉ gắn vào một EC2 trong một AZ. Dữ liệu không mất khi EC2 restart. Có nhiều loại: SSD nhanh (gp3, io2) cho database, HDD rẻ (st1, sc1) cho dữ liệu lớn ít đọc. Xem [EBS Volume Types](glossary.md#ebs-volume-types).

### EFS
**Elastic File System** — Ổ đĩa mạng dùng chung cho Linux. Nhiều EC2, container, Lambda có thể đọc/ghi cùng lúc vào cùng một file system. Tự động mở rộng dung lượng. Phù hợp khi nhiều server cần chia sẻ file.

### FSx
Các file system được AWS quản lý hoàn toàn, dành cho nhu cầu đặc thù:
- **FSx for Windows**: file share cho môi trường Windows/Active Directory (dùng SMB protocol).
- **FSx for Lustre**: file system hiệu năng cao cho HPC, ML, xử lý video.
- **FSx for NetApp ONTAP / OpenZFS**: cho enterprise cần tính năng nâng cao.

### AWS Backup
Dịch vụ backup tập trung cho nhiều loại resource AWS (EBS, RDS, DynamoDB, EFS, FSx...). Thay vì cấu hình backup từng dịch vụ riêng lẻ, bạn tạo một policy duy nhất áp dụng cho tất cả. Hỗ trợ cross-account và cross-region.

### Storage Gateway
Thiết bị (hoặc VM) đặt tại on-premises, hoạt động như một cầu nối lên AWS:
- **File Gateway**: server on-prem vẫn dùng NFS/SMB bình thường, dữ liệu tự lên S3.
- **Volume Gateway**: ổ đĩa on-prem được backup lên AWS.
- **Tape Gateway**: thay thế băng từ (tape) bằng S3 Glacier, không cần đổi phần mềm backup.

---

## Database

### RDS
**Relational Database Service** — Database SQL được AWS quản lý hoàn toàn. Hỗ trợ MySQL, PostgreSQL, MariaDB, Oracle, SQL Server. AWS lo patch, backup tự động, failover. Bạn chỉ lo query và schema.

### Aurora
Database quan hệ do AWS tự xây dựng, tương thích với MySQL và PostgreSQL nhưng nhanh hơn và có availability cao hơn. Storage tự mở rộng, tự replicate 6 bản sang 3 AZ. Có phiên bản Serverless tự điều chỉnh capacity.

### DynamoDB
Database NoSQL serverless của AWS. Lưu dữ liệu dạng key-value và document. Scale tự động đến hàng triệu request/giây. Không có server cần quản lý. Phù hợp cho ứng dụng cần scale lớn, traffic biến động. Xem [Từ Điển → DAX](glossary.md#dax) cho cache.

### ElastiCache
Cache in-memory được AWS quản lý. Lưu dữ liệu hay dùng vào RAM để đọc cực nhanh (microsecond), giảm tải cho database. Có hai engine: **Redis/Valkey** (nhiều tính năng, hỗ trợ persistence) và **Memcached** (đơn giản, scale ngang).

### Redshift
Data warehouse của AWS — kho dữ liệu phân tích quy mô lớn. Khác RDS ở chỗ Redshift tối ưu cho query tổng hợp trên hàng tỷ dòng (OLAP), không phải giao dịch thời gian thực (OLTP). Dùng cho BI, báo cáo, phân tích xu hướng. Xem [Từ Điển → OLAP](glossary.md#olap).

---

## Integration

### SQS
**Simple Queue Service** — Hàng đợi tin nhắn. App A gửi message vào queue, app B lấy ra xử lý theo tốc độ riêng. Khi B bị chậm hoặc crash, message không mất — chờ trong queue. Dùng để tách rời (decouple) hai thành phần hệ thống. Xem [Từ Điển → DLQ](glossary.md#dlq).

### SNS
**Simple Notification Service** — Pub/sub: một thông báo gửi đến nhiều nơi cùng lúc. Ví dụ: đơn hàng mới → SNS → gửi email cho khách + cập nhật inventory + ghi log analytics, tất cả song song. Khác SQS: SNS push ngay, không lưu message.

### EventBridge
Bus sự kiện (event bus) kết nối các service với nhau theo rule. Ví dụ: khi EC2 instance bị dừng → tự động gửi cảnh báo Slack. Tích hợp được với hàng chục dịch vụ SaaS (Salesforce, Zendesk...) và AWS service.

### Step Functions
Công cụ xây dựng workflow nhiều bước có logic phức tạp: rẽ nhánh (if/else), chạy song song, retry tự động khi lỗi, chờ phê duyệt từ người. Dùng thay cho Lambda gọi Lambda lồng nhau — dễ debug hơn nhiều.

### API Gateway
Cổng vào cho API của bạn. Nhận HTTP request từ client, chuyển đến Lambda/EC2/service backend. Xử lý authentication, rate limiting, caching, logging. Hỗ trợ REST, HTTP, và WebSocket API.

### AppSync
API GraphQL được AWS quản lý. Thay vì client phải gọi nhiều endpoint REST, GraphQL cho phép client chỉ lấy đúng dữ liệu cần trong một request. Hỗ trợ realtime subscription (như chat, live dashboard).

---

## Observability & Governance

### CloudWatch
Hệ thống giám sát trung tâm của AWS. Thu thập metric (CPU, memory, latency...), log từ mọi dịch vụ, thiết lập alarm khi vượt ngưỡng, tự động trigger action (scale up, gửi thông báo). Mặc định không có metric memory/disk của EC2 — cần cài CloudWatch Agent.

### CloudTrail
Nhật ký ghi lại mọi API call trên AWS account: ai làm gì, lúc nào, từ IP nào. Dùng để audit, điều tra bảo mật, và đáp ứng compliance. Không theo dõi performance — đó là việc của CloudWatch.

### Config
Theo dõi lịch sử thay đổi cấu hình của resource AWS. Trả lời câu hỏi "security group này từng mở port 22 ra internet lúc nào?". Có thể đặt rule kiểm tra compliance tự động và cảnh báo khi vi phạm.

### Systems Manager
Bộ công cụ quản lý EC2 và server on-premises từ xa:
- **Session Manager**: kết nối SSH vào EC2 không cần mở port 22.
- **Patch Manager**: tự động vá bảo mật theo lịch.
- **Parameter Store**: lưu cấu hình (connection string, feature flag) an toàn.
- **Run Command**: chạy lệnh trên nhiều server cùng lúc.

### Trusted Advisor
Cố vấn tự động kiểm tra account và đưa ra gợi ý tiết kiệm chi phí, tăng bảo mật, cải thiện performance. Ví dụ: cảnh báo security group đang mở SSH cho cả internet, hoặc EC2 instance không dùng mấy tháng nay.

### Compute Optimizer
Phân tích lịch sử sử dụng EC2, EBS, Lambda để gợi ý instance type phù hợp hơn. Giúp phát hiện: instance quá to so với nhu cầu (lãng phí) hoặc quá nhỏ (thiếu năng lực).

---

## Security Services

### KMS
**Key Management Service** — Trung tâm quản lý khóa mã hóa. Tạo, lưu, và kiểm soát việc dùng encryption key. Mọi dịch vụ AWS (S3, EBS, RDS...) dùng KMS để mã hóa dữ liệu at rest. Xem [Từ Điển → KMS](glossary.md#kms).

### ACM
**AWS Certificate Manager** — Cấp và tự động gia hạn TLS/HTTPS certificate miễn phí. Gắn vào ALB, CloudFront, API Gateway để bật HTTPS. Xem [Từ Điển → ACM](glossary.md#acm).

### Secrets Manager
Lưu và tự động xoay vòng (rotate) mật khẩu/API key nhạy cảm: database password, API key bên thứ ba. App đọc secret qua SDK thay vì hardcode vào code. Rotation tự động theo lịch.

### WAF
**Web Application Firewall** — Lá chắn chặn các cuộc tấn công web phổ biến: SQL injection, XSS, bad bot, request tốc độ cao bất thường. Gắn vào CloudFront, ALB, hoặc API Gateway. Xem [Từ Điển → WAF](glossary.md#waf).

### Shield
Bảo vệ DDoS cho ứng dụng trên AWS:
- **Shield Standard**: miễn phí, tự động bảo vệ cơ bản cho tất cả tài nguyên.
- **Shield Advanced**: trả phí, bảo vệ tinh vi hơn, đội AWS hỗ trợ 24/7 khi bị tấn công, và hoàn tiền phí AWS phát sinh do DDoS.

### Cognito
Hệ thống xác thực người dùng cho ứng dụng web/mobile của bạn. Thay vì tự xây login/register, Cognito lo hết: đăng ký, đăng nhập, quên mật khẩu, MFA, và đăng nhập qua Google/Facebook/Apple.

### GuardDuty
Phát hiện mối đe dọa bảo mật tự động bằng machine learning. Phân tích log VPC Flow, DNS, CloudTrail để tìm dấu hiệu bất thường: tài khoản bị compromise, cryptomining, data exfiltration. Bật ngay mà không cần cài agent.

### Macie
Tự động quét và phân loại dữ liệu nhạy cảm trong S3: số thẻ tín dụng, số CMND, thông tin y tế... Cảnh báo khi phát hiện bucket S3 chứa PII (thông tin cá nhân) bị public hoặc chia sẻ không đúng.

### Inspector
Quét lỗ hổng bảo mật (vulnerability) tự động trong EC2, Lambda, và container image. Kết quả được xếp hạng theo mức độ nguy hiểm và đề xuất cách vá. Chạy liên tục, không phải chỉ theo lịch.

### Security Hub
Bảng điều khiển tập trung tổng hợp cảnh báo bảo mật từ nhiều nguồn: GuardDuty, Macie, Inspector, và cả công cụ bên thứ ba. Giúp team security có một chỗ nhìn toàn cảnh thay vì vào từng service riêng.

---

## Migration & Transfer

### DataSync
Công cụ chuyển file/object tự động và nhanh giữa on-premises và AWS (S3, EFS, FSx). Tự xử lý checksum, retry, và bandwidth throttling. Nhanh hơn tự viết script rsync.

### DMS
**Database Migration Service** — Dịch vụ di chuyển database lên AWS. Hỗ trợ migration một lần hoặc liên tục (CDC — change data capture) để giảm downtime xuống gần bằng 0. Dùng kèm SCT khi đổi engine database. Xem [Từ Điển → CDC](glossary.md#cdc).

### Snow Family
Thiết bị phần cứng vật lý để chuyển lượng dữ liệu khổng lồ (terabyte đến petabyte) khi đường internet quá chậm:
- **Snowcone**: nhỏ gọn, 8-14TB.
- **Snowball Edge**: 80TB, có thể chạy Lambda/EC2 tại chỗ.
- **Snowmobile**: container xe tải, 100PB.

### Transfer Family
Endpoint quản lý cho giao thức truyền file SFTP, FTPS, FTP — kết nối thẳng vào S3 hoặc EFS. Dùng khi partner/khách hàng cần upload file bằng SFTP mà không muốn thay đổi workflow của họ.

---

## Analytics (Nên Biết)

### Athena
Query SQL trực tiếp trên file trong S3 — không cần load vào database. Trả tiền theo lượng data scan. Dùng cho phân tích log, ad-hoc query data lake. Xem [Từ Điển → OLAP](glossary.md#olap).

### Glue
Dịch vụ ETL (Extract-Transform-Load) serverless. Thu thập dữ liệu từ nhiều nguồn, biến đổi format (CSV → Parquet), và nạp vào S3/Redshift. Kèm Data Catalog lưu metadata schema. Xem [Từ Điển → ETL](glossary.md#etl).

### Kinesis
Family dịch vụ xử lý dữ liệu stream (luồng) thời gian thực:
- **Kinesis Data Streams**: nhận và lưu stream, nhiều consumer đọc độc lập.
- **Kinesis Data Firehose** (Amazon Data Firehose): nhận stream và tự động đưa vào S3/Redshift/OpenSearch, không cần code consumer.
- **Kinesis Video Streams**: stream video.

### EMR
**Elastic MapReduce** — Cluster Hadoop/Spark được AWS quản lý. Dùng cho xử lý dữ liệu lớn cần kiểm soát chi tiết: chạy Spark job, Hive query, custom ETL phức tạp. Bạn vẫn quản lý cluster.

### Lake Formation
Dịch vụ xây dựng và quản lý data lake trên S3. Lo phần phức tạp: import dữ liệu, phân loại, và đặc biệt là **kiểm soát quyền truy cập** ở cấp cột/hàng. Ai được xem cột nào, bảng nào.

### QuickSight
Công cụ BI (Business Intelligence) và dashboard của AWS. Kết nối với Athena, Redshift, S3, RDS... để tạo biểu đồ, báo cáo và chia sẻ với stakeholder mà không cần code.

### OpenSearch
Search engine và analytics mạnh mẽ (fork của Elasticsearch) được AWS quản lý. Dùng để tìm kiếm full-text, phân tích log, theo dõi metric thời gian thực. Thường đi kèm Kibana-compatible dashboard.

### MSK
**Managed Streaming for Apache Kafka** — Kafka được AWS quản lý. Dùng khi hệ thống đã xây trên Kafka và muốn chạy trên AWS mà không tự vận hành broker, ZooKeeper, replication.

---

## Specialty Databases (Nên Biết)

### DocumentDB
Database document (JSON) compatible với MongoDB. Dùng khi app đang dùng MongoDB và muốn chuyển sang AWS managed service mà ít thay đổi code nhất.

### Neptune
Database đồ thị (graph database). Tối ưu cho dữ liệu có nhiều mối quan hệ phức tạp: mạng xã hội, fraud detection (phát hiện gian lận), recommendation engine, knowledge graph.

### Keyspaces
Apache Cassandra compatible, được AWS quản lý. Dùng khi app đang dùng Cassandra cần scale write cực lớn theo kiểu NoSQL wide-column.

### QLDB
**Quantum Ledger Database** — Database có lịch sử thay đổi bất biến, không thể sửa/xóa record cũ. Có cryptographic verification để chứng minh dữ liệu không bị can thiệp. Dùng cho hệ thống tài chính, chuỗi cung ứng cần audit trail tuyệt đối.

---

## Các Dịch Vụ Khác (Nên Biết)

### AppFlow
Kết nối dữ liệu giữa dịch vụ SaaS (Salesforce, ServiceNow, Slack...) và AWS (S3, Redshift) theo lịch hoặc trigger, không cần code integration thủ công.

### Amazon MQ
Message broker được AWS quản lý, hỗ trợ giao thức AMQP (RabbitMQ) và OpenWire (ActiveMQ). Dùng khi app đang dùng RabbitMQ/ActiveMQ và muốn lift-and-shift lên cloud mà không đổi code.

### Elastic Beanstalk
Nền tảng deploy app nhanh: bạn upload code (Java, Python, Node, PHP...), AWS tự lo EC2, ALB, ASG, RDS. Không phải serverless — vẫn dùng EC2 bên dưới. Dùng khi muốn deploy nhanh mà không muốn tự cấu hình hạ tầng.

### Batch
Dịch vụ chạy batch job (tác vụ hàng loạt) theo hàng đợi. Tự động cấp phát compute khi có job, giải phóng khi xong. Kết hợp Spot Instances để tiết kiệm chi phí. Dùng cho rendering, ML training, ETL lớn.

### Outposts
Rack server AWS đặt tại data center của bạn. Chạy dịch vụ AWS (EC2, EBS, RDS, EKS...) ngay tại chỗ để đáp ứng yêu cầu latency cực thấp hoặc data residency (dữ liệu không được rời khỏi cơ sở).

### Wavelength
AWS compute và storage đặt trong data center của nhà mạng viễn thông (Verizon, KDDI...). Giúp app 5G có độ trễ dưới 10ms — không thể đạt được nếu phải gửi data về Region thông thường.

### VMware Cloud on AWS
Chạy workload VMware vSphere trực tiếp trên hạ tầng AWS mà không cần refactor. Dùng cho migration từ VMware on-premises sang cloud mà giữ nguyên công cụ quản lý VMware.

### CloudFormation
Viết hạ tầng bằng file template (JSON/YAML) — gọi là Infrastructure as Code (IaC). Một template mô tả toàn bộ stack (VPC, EC2, RDS, SG...), CloudFormation tự tạo và quản lý lifecycle. Thay đổi template → CloudFormation update hạ tầng.

### Service Catalog
Cho phép admin tạo danh mục "sản phẩm" được phê duyệt (CloudFormation template đã qua review) và cho phép developer tự deploy từ danh mục đó. Đảm bảo mọi resource đều đúng chuẩn bảo mật/chi phí của tổ chức.

### Well-Architected Tool
Công cụ review kiến trúc theo 6 trụ cột (pillar) của AWS Well-Architected Framework: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, Sustainability. Trả lời câu hỏi → nhận gợi ý cải thiện.

---

## AI/ML Services (Nên Biết — Mức Use Case)

> Với SAA-C03, chỉ cần biết **dịch vụ này làm gì** — không cần biết cách train model.

| Dịch vụ | Làm gì |
|---|---|
| **Comprehend** | Phân tích văn bản: cảm xúc (tích cực/tiêu cực), entity (tên người/địa danh), ngôn ngữ |
| **Polly** | Chuyển văn bản thành giọng nói (Text-to-Speech) |
| **Rekognition** | Nhận diện khuôn mặt, vật thể, nội dung không phù hợp trong ảnh/video |
| **Textract** | Trích xuất text, bảng, form từ tài liệu scan (PDF, ảnh) |
| **Transcribe** | Chuyển giọng nói thành văn bản (Speech-to-Text), hỗ trợ tiếng Việt |
| **Translate** | Dịch thuật tự động giữa các ngôn ngữ |
| **SageMaker** | Nền tảng build, train, và deploy ML model; dùng khi cần train model riêng |
| **Lex** | Xây chatbot hội thoại (giọng nói + text), cùng công nghệ với Alexa |
| **Forecast** | Dự báo chuỗi thời gian (doanh thu, nhu cầu tồn kho) |
| **Fraud Detector** | Phát hiện gian lận dựa trên pattern lịch sử |
| **Kendra** | Tìm kiếm doanh nghiệp: hỏi câu hỏi tự nhiên, nhận câu trả lời từ tài liệu nội bộ |

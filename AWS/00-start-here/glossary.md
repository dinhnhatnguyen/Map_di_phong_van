# Từ Điển — AWS SAA-C03 Glossary

Giải thích các từ viết tắt và khái niệm xuất hiện trong bộ tài liệu này. Nhấp vào link từ bất kỳ file nào để tra cứu ở đây.

---

## Dịch Vụ & Viết Tắt AWS

### ACM

**AWS Certificate Manager** — Dịch vụ cấp và quản lý TLS/SSL certificate miễn phí. Dùng với ALB, CloudFront, API Gateway. Lưu ý: certificate cho CloudFront **phải** tạo ở Region `us-east-1`.

### ALB

**Application Load Balancer** — Load balancer hoạt động ở tầng 7 (HTTP/HTTPS). Hỗ trợ routing theo path (`/api/*`), hostname, header, và query string. Dùng cho web app, container, Lambda target. Xem thêm: [NLB](#nlb), [GWLB](#gwlb).

### ASG

**Auto Scaling Group** — Nhóm EC2 instance tự động tăng/giảm số lượng theo policy. Kết hợp với ALB/NLB để đạt [HA](#ha). Có bốn loại policy: target tracking, step scaling, scheduled, predictive.

### CDN

**Content Delivery Network** — Mạng phân tán các edge server toàn cầu để cache nội dung gần người dùng hơn, giảm latency. CloudFront là CDN của AWS.

### CDC

**Change Data Capture** — Kỹ thuật bắt mọi thay đổi (INSERT/UPDATE/DELETE) trong database theo thời gian thực, thay vì dump toàn bộ. AWS DMS dùng CDC để migrate database với downtime tối thiểu.

### CIDR

**Classless Inter-Domain Routing** — Cách biểu diễn dải địa chỉ IP. Ví dụ `10.0.0.0/16` là 65,536 địa chỉ (VPC thường dùng /16); `10.0.1.0/24` là 256 địa chỉ (subnet thường dùng /24). Con số sau `/` càng lớn thì dải càng nhỏ.

### DAX

**DynamoDB Accelerator** — Cache in-memory purpose-built cho DynamoDB. Giảm read latency từ millisecond xuống microsecond. API tương thích với DynamoDB — app không cần đổi code nhiều. Không giúp write-heavy workload.

### DDoS

**Distributed Denial of Service** — Kiểu tấn công từ nhiều nguồn đồng thời để làm quá tải và tê liệt dịch vụ. AWS Shield Standard bảo vệ mặc định miễn phí; Shield Advanced cho bảo vệ nâng cao có tính phí.

### DLQ

**Dead Letter Queue** — Queue SQS phụ nhận các message xử lý thất bại sau `maxReceiveCount` lần thử. Dùng để debug và đảm bảo không mất message lỗi vĩnh viễn.

### DMS

**AWS Database Migration Service** — Dịch vụ migrate database lên AWS với minimal downtime. Hỗ trợ [CDC](#cdc) (ongoing replication) để keep source và target đồng bộ trong quá trình migration. Dùng kèm [SCT](#sct) cho heterogeneous migration.

### DNS

**Domain Name System** — Hệ thống dịch tên miền (`example.com`) sang địa chỉ IP. Giống danh bạ điện thoại của internet. Route 53 là managed DNS service của AWS.

### EBS

**Elastic Block Store** — Block storage (ổ đĩa ảo) gắn với EC2. Tồn tại trong **một AZ** duy nhất. Dữ liệu tồn tại độc lập với vòng đời EC2 instance (trừ khi bật `DeleteOnTermination`). Có nhiều loại: [gp3, io2, st1, sc1](#ebs-volume-types).

### EBS Volume Types

Các loại volume EBS và khi nào dùng:

| Type | Loại | Dùng khi                                      |
| ---- | ---- | --------------------------------------------- |
| gp3  | SSD  | Default tốt, cấu hình IOPS/throughput độc lập |
| gp2  | SSD  | Legacy, burst theo dung lượng                 |
| io2  | SSD  | Mission-critical, IOPS cao, latency thấp (DB) |
| st1  | HDD  | Big data, sequential throughput cao           |
| sc1  | HDD  | Cold data, ít truy cập, cost thấp nhất        |

### EC2

**Elastic Compute Cloud** — Máy chủ ảo (virtual machine) trên AWS. Bạn chọn OS, instance type, network, storage. Mức kiểm soát cao nhất trong các compute option.

### ECR

**Elastic Container Registry** — Private Docker image registry trên AWS. Lưu container image, tích hợp IAM, image scanning, lifecycle policy. Dùng với [ECS](#ecs), [EKS](#eks), Lambda container image.

### ECS

**Elastic Container Service** — Dịch vụ chạy container Docker của AWS. Có hai launch type: **EC2** (bạn quản lý EC2 worker node) và **Fargate** (AWS quản lý hạ tầng, bạn chỉ lo container).

### EFS

**Elastic File System** — Managed NFS (Network File System) cho Linux. Có thể mount từ nhiều EC2/ECS task/Lambda đồng thời, tự scale dung lượng. Khác với [EBS](#ebs) chỉ gắn được một instance trong một AZ.

### EKS

**Elastic Kubernetes Service** — Managed Kubernetes control plane trên AWS. AWS lo patch và scale control plane; bạn lo worker node (managed node group, self-managed, hoặc Fargate).

### ENI

**Elastic Network Interface** — Card mạng ảo trong VPC. Mỗi EC2 instance có ít nhất một ENI với private IP. [Security Group](#security-group) gắn vào ENI, không gắn vào instance.

### ETL

**Extract, Transform, Load** — Quy trình: (1) trích xuất dữ liệu từ nguồn, (2) biến đổi format/schema, (3) nạp vào data warehouse hoặc data lake. AWS Glue là managed ETL service.

### FSx

**Amazon FSx** — Tên chung cho family managed file system của AWS:

- **FSx for Windows File Server**: SMB, Active Directory
- **FSx for Lustre**: HPC, ML, high throughput, link với S3
- **FSx for NetApp ONTAP**: enterprise NFS/SMB/iSCSI
- **FSx for OpenZFS**: OpenZFS managed

### GWLB

**Gateway Load Balancer** — Load balancer Layer 3/4 dùng để triển khai fleet appliance (firewall, IDS/IPS) theo cách transparent. Traffic đi qua appliance trước khi tới destination mà không cần đổi IP hay config app.

### HSM

**Hardware Security Module** — Thiết bị phần cứng chuyên dụng lưu và xử lý cryptographic key an toàn, tách biệt hoàn toàn khỏi phần mềm. AWS CloudHSM cung cấp dedicated HSM đạt [FIPS](#fips) 140-2 Level 3.

### IAM

**Identity and Access Management** — Dịch vụ quản lý identity và permission trên AWS. Bao gồm: User, Group, Role, Policy. Rule quan trọng: explicit deny thắng mọi allow.

### IGW

**Internet Gateway** — Cổng kết nối VPC với internet. Một VPC có tối đa một IGW. Public subnet có route `0.0.0.0/0 → IGW`; private subnet không có route này.

### IOPS

**Input/Output Operations Per Second** — Số thao tác đọc/ghi mỗi giây của storage. Quan trọng cho database (EBS io2 cho IOPS cao). Khác với [throughput](#throughput) đo bằng MB/s.

### KMS

**Key Management Service** — Dịch vụ tạo và quản lý encryption key. Tích hợp với S3, EBS, RDS, Lambda, và nhiều service khác. Đạt [FIPS](#fips) 140-2 Level 2. Dùng envelope encryption: KMS key mã hóa data key, data key mã hóa dữ liệu.

### LSI

**Local Secondary Index** — Index phụ của DynamoDB table, dùng cùng partition key với table nhưng sort key khác. **Chỉ tạo được khi tạo table**, không thể thêm sau. Hỗ trợ strongly consistent read.

### GSI

**Global Secondary Index** — Index phụ của DynamoDB với partition key và sort key **hoàn toàn khác** với table. Có thể **tạo sau** khi table đã tồn tại. Chỉ hỗ trợ eventual consistency.

### MFA

**Multi-Factor Authentication** — Xác thực hai yếu tố: password + thiết bị thứ hai (authenticator app, hardware key). Tăng cường bảo mật tài khoản AWS.

### MSK

**Managed Streaming for Apache Kafka** — Managed Apache Kafka trên AWS. Dùng khi workload cần Kafka API/protocol/ecosystem mà không muốn tự vận hành Kafka cluster.

### NAT

**Network Address Translation** — Kỹ thuật cho phép instance trong private subnet gửi traffic ra internet outbound mà không expose địa chỉ IP private. NAT Gateway là managed service; đặt mỗi AZ để đạt HA.

### NFS

**Network File System** — Protocol chia sẻ file trên Linux/Unix. [EFS](#efs) dùng NFS 4.1. Cho phép nhiều client mount cùng một file system đồng thời.

### NLB

**Network Load Balancer** — Load balancer Layer 4 (TCP/UDP/TLS). Ultra-low latency, hỗ trợ static Elastic IP, xử lý hàng triệu request/giây. Dùng cho non-HTTP app, gaming, high-throughput TCP.

### OAC / OAI

**Origin Access Control / Origin Access Identity** — Cơ chế để CloudFront truy cập S3 bucket private mà không cần public bucket. OAC là phiên bản mới hơn, được khuyến nghị thay OAI. Bucket policy chỉ allow `cloudfront.amazonaws.com` principal.

### OLAP

**Online Analytical Processing** — Loại workload phân tích dữ liệu lớn: query phức tạp, aggregation, reporting. Ví dụ: Redshift, Athena. **Không** phải giao dịch thời gian thực. Xem thêm: [OLTP](#oltp).

### OLTP

**Online Transaction Processing** — Loại workload giao dịch thời gian thực: nhiều INSERT/UPDATE/DELETE nhỏ, latency thấp. Ví dụ: RDS, Aurora, DynamoDB. Ngược lại với [OLAP](#olap).

### RDS

**Relational Database Service** — Managed SQL database. Hỗ trợ MySQL, PostgreSQL, MariaDB, Oracle, SQL Server. AWS lo patching, backup, replication. Bạn chọn instance type và storage.

### RPO

**Recovery Point Objective** — Lượng dữ liệu tối đa có thể chấp nhận mất khi xảy ra thảm họa, đo bằng thời gian. RPO = 1 giờ nghĩa là tối đa mất 1 giờ dữ liệu gần nhất. Càng nhỏ càng tốt, càng tốn kém.

### RTO

**Recovery Time Objective** — Thời gian tối đa có thể chấp nhận để khôi phục dịch vụ sau thảm họa. RTO = 4 giờ nghĩa là dịch vụ phải hoạt động lại trong 4 giờ. Càng nhỏ càng tốt, càng tốn kém.

### S3

**Simple Storage Service** — Object storage của AWS. Lưu file dưới dạng object (không phải file system [POSIX](#posix)). Durability 11 nines (99.999999999%). Không giới hạn dung lượng. Không mount như ổ đĩa bình thường.

### SCT

**AWS Schema Conversion Tool** — Công cụ chuyển đổi database schema và stored procedure từ engine này sang engine khác (ví dụ Oracle → Aurora PostgreSQL). Dùng kèm [DMS](#dms) cho heterogeneous migration.

### SCP

**Service Control Policy** — Policy trong AWS Organizations **giới hạn maximum permission** cho account hoặc OU. SCP không cấp quyền, chỉ thu hẹp quyền tối đa. **Không** ảnh hưởng root user và service-linked roles.

### SMB

**Server Message Block** — Protocol chia sẻ file và printer phổ biến trên Windows. FSx for Windows File Server dùng SMB. Khác với [NFS](#nfs) dùng cho Linux.

### SNS

**Simple Notification Service** — Pub/sub messaging. Topic push message đến subscriber (SQS, Lambda, email, HTTP/S, SMS). Không lưu message như queue — nếu subscriber không nhận được thì mất.

### SQS

**Simple Queue Service** — Message queue. Producer gửi message vào queue, consumer pull và xử lý theo tốc độ riêng. Standard queue (high throughput, at-least-once) hoặc FIFO (ordering, exactly-once).

### SSO

**Single Sign-On** — Đăng nhập một lần dùng được nhiều hệ thống. AWS IAM Identity Center là managed SSO cho workforce access nhiều AWS account.

### STS

**Security Token Service** — Dịch vụ cấp temporary credential (access key + secret + session token). Dùng khi assume role cross-account hoặc federated identity.

### TLS / SSL

**Transport Layer Security / Secure Sockets Layer** — Giao thức mã hóa kết nối mạng. SSL là tên cũ, TLS là phiên bản hiện đại. HTTPS dùng TLS. ACM quản lý TLS certificate.

### VPC

**Virtual Private Cloud** — Mạng riêng ảo trong AWS Region. Bạn kiểm soát CIDR, subnet, route table, firewall. Mỗi account có default VPC. VPC **không** span nhiều Region.

### WAF

**Web Application Firewall** — Firewall ở tầng ứng dụng. Chặn SQL injection, XSS, rate-based attack, bad bot. Gắn vào CloudFront, ALB, hoặc API Gateway.

---

## Khái Niệm Kiến Trúc

### Active-Active

Cả hai (hoặc nhiều) region/AZ đều phục vụ traffic đồng thời. [RTO](#rto)/[RPO](#rpo) gần bằng 0 nhưng cost cao nhất trong bốn chiến lược DR. Ví dụ: DynamoDB Global Tables, Aurora Global Database, Multi-Region ALB.

### Active-Passive

Một environment chính xử lý toàn bộ traffic; environment dự phòng chờ sẵn (standby). Khi primary fail, traffic failover sang secondary. Rẻ hơn active-active. Route 53 Failover routing hỗ trợ pattern này.

### AZ — Availability Zone

Data center (hoặc cụm data center) vật lý riêng biệt trong một AWS Region. Mỗi Region có ít nhất 3 AZ. Các AZ kết nối qua đường mạng tốc độ cao, cách nhau đủ xa để một thảm họa không ảnh hưởng nhiều AZ.

### Blue-Green Deployment

Kỹ thuật deploy không downtime: chạy version mới (green) song song version cũ (blue), chuyển traffic sang green dần hoặc toàn bộ, rollback dễ dàng nếu lỗi.

### Canary Deployment

Deploy version mới cho một phần nhỏ user trước (ví dụ 5%), quan sát metric/lỗi, rồi tăng dần. Route 53 Weighted routing dùng cho canary ở DNS level.

### Cold Start

Độ trễ lần đầu tiên khi Lambda function được gọi mà chưa có execution environment sẵn sàng. AWS phải khởi tạo container, nạp code, chạy init. Giảm bằng Provisioned Concurrency.

### Decouple / Loose Coupling

Thiết kế các thành phần hệ thống không phụ thuộc trực tiếp nhau. Dùng SQS/SNS/EventBridge làm buffer trung gian. Khi một service chậm hoặc down, service khác không bị kéo theo.

### Envelope Encryption

Kỹ thuật mã hóa hai lớp: (1) data key mã hóa dữ liệu thực, (2) KMS master key mã hóa data key. Data key lưu cùng dữ liệu đã mã hóa (ở dạng encrypted). KMS chỉ thao tác với key nhỏ, không với toàn bộ data.

### Eventual Consistency

Đọc dữ liệu có thể trả về giá trị cũ trong khoảng thời gian rất ngắn sau khi write thành công. DynamoDB mặc định là eventual consistency. Nhanh hơn và rẻ hơn strongly consistent.

### Fan-Out

Pattern một message/event được push đến **nhiều consumer song song độc lập**. SNS topic publish → nhiều SQS queue, mỗi queue xử lý riêng. Các consumer không biết nhau và không ảnh hưởng nhau.

### Fault Tolerance

Khả năng hệ thống tiếp tục hoạt động bình thường ngay cả khi có thành phần bị lỗi, không cần failover hay can thiệp. Cao hơn HA (HA có thể có thời gian switch ngắn).

### FIPS

**Federal Information Processing Standard** — Chuẩn bảo mật của chính phủ Mỹ cho module mật mã. **Level 2** là minimum an toàn (KMS đạt được). **Level 3** yêu cầu phần cứng chuyên biệt chống tamper (CloudHSM đạt được). Thường xuất hiện trong đề compliance.

### HA — High Availability

Khả năng hệ thống tiếp tục hoạt động khi có thành phần bị lỗi. Đạt bằng redundancy: Multi-AZ (ASG, RDS Multi-AZ), nhiều instance sau load balancer, health check tự động.

### Horizontal Scaling

Thêm nhiều instance để xử lý tải (scale out) hoặc bớt đi (scale in). Phù hợp cho stateless app. ASG thực hiện horizontal scaling tự động.

### Hot Partition

Vấn đề trong DynamoDB khi **một partition key** nhận quá nhiều traffic so với các key khác, gây throttling cho toàn bộ bảng. Xảy ra khi partition key thiếu cardinality (ví dụ: dùng `date` làm partition key, mọi write trong ngày đổ vào một partition).

### Idempotent

Operation thực hiện nhiều lần cho kết quả giống hệt như một lần. Quan trọng với SQS at-least-once delivery: consumer phải xử lý idempotent vì message có thể bị giao nhiều lần.

### Lift-and-Shift

Chiến lược migration: chuyển workload từ on-premises lên AWS mà không đổi kiến trúc (as-is). Nhanh, ít rủi ro, nhưng chưa tối ưu cho cloud. Dùng Application Migration Service.

### Managed Service

Dịch vụ AWS lo hạ tầng bên dưới: patching, scaling, backup, HA. Ví dụ: RDS, DynamoDB, SQS, Lambda. Ngược lại là self-managed (cài database lên EC2 tự chăm sóc).

### Multi-AZ

Triển khai resource trên nhiều Availability Zone trong **cùng Region**. Nếu một AZ fail, resource ở AZ khác tiếp tục hoạt động. Không bảo vệ khỏi Region failure.

### Multi-Region

Triển khai resource trên nhiều AWS Region để DR hoặc global low latency. Tốn kém hơn Multi-AZ. Dùng khi requirement DR nghiêm ngặt ([RTO](#rto)/[RPO](#rpo) rất thấp) hoặc phục vụ user toàn cầu.

### Pilot Light

Chiến lược DR: chỉ duy trì **core data layer** (database) luôn chạy ở secondary region, compute tắt. Khi failover cần "bật" compute. [RTO](#rto) trung bình (phút đến giờ). Rẻ hơn Warm Standby.

### POSIX

**Portable Operating System Interface** — Chuẩn interface file system trên Unix/Linux (đường dẫn, permission, inode, symlink...). EFS là POSIX-compliant. S3 **không** phải — không có directory thực sự, không có permission POSIX.

### Provisioned Concurrency

Lambda feature giữ sẵn execution environment ở trạng thái "warm", loại bỏ [cold start](#cold-start). Tốn thêm chi phí kể cả khi function không chạy.

### Region

Khu vực địa lý AWS triển khai cụm data center. Mỗi Region độc lập hoàn toàn (us-east-1, ap-southeast-1, eu-west-1...). Resource trong Region **không tự replicate** sang Region khác trừ khi bạn cấu hình.

### Security Group

Firewall **stateful** gắn vào ENI/instance. Chỉ có rule allow (không có deny). Stateful = return traffic tự động được phép mà không cần rule outbound. Mặc định deny all inbound, allow all outbound.

### Serverless

Mô hình không quản lý server: AWS tự scale, patch, allocate resource. Bạn chỉ lo code/config. Ví dụ: Lambda, Fargate, DynamoDB, Aurora Serverless, S3, SQS, API Gateway. Trả tiền theo usage, không phải capacity.

### Stateful vs Stateless

- **Stateful**: app lưu session/state trong memory của instance. Khó scale ngang vì request tiếp theo phải đến đúng instance cũ.
- **Stateless**: app không lưu state local. Session externalize ra ElastiCache/DynamoDB. Scale ngang dễ, load balancer gửi request đến bất kỳ instance nào cũng được.

### Strongly Consistent Read

Đọc DynamoDB luôn trả về giá trị mới nhất ngay sau write thành công. Tốn gấp đôi [RCU](#rcu) so với eventually consistent. Chỉ áp dụng cho table và LSI.

### Throughput

Lượng dữ liệu xử lý được trong một đơn vị thời gian (MB/s hoặc GB/s). Quan trọng cho storage sequential (EBS st1), network, ETL. Khác với [IOPS](#iops) đo số thao tác.

### Tight Coupling

Thiết kế các service phụ thuộc trực tiếp nhau, ví dụ Lambda A gọi Lambda B đồng bộ. Khi B chậm thì A chậm theo; khi B down thì A lỗi. Giải pháp: dùng [loose coupling](#decouple--loose-coupling) qua SQS/SNS.

### Visibility Timeout

Khoảng thời gian SQS ẩn message sau khi consumer nhận, cho consumer thời gian xử lý. **Default: 30 giây. Max: 12 giờ.** Phải lớn hơn thời gian xử lý thực tế của consumer. Nếu timeout hết mà consumer chưa xóa message, message hiện lại để consumer khác nhận (dễ gây duplicate).

### Warm Standby

Chiến lược DR: một environment thu nhỏ **(reduced capacity)** luôn chạy ở secondary region. Khi failover chỉ cần scale up, không cần bật từ đầu. [RTO](#rto) thấp hơn [Pilot Light](#pilot-light). Cost cao hơn Pilot Light.

---

## Nhanh Tay Tra Cứu

| Thấy từ này                | Xem mục                                                     |
| -------------------------- | ----------------------------------------------------------- |
| AZ                         | [AZ](#az--availability-zone)                                |
| ALB / NLB / GWLB           | [ALB](#alb) · [NLB](#nlb) · [GWLB](#gwlb)                   |
| ASG                        | [ASG](#asg)                                                 |
| CIDR                       | [CIDR](#cidr)                                               |
| CDC                        | [CDC](#cdc)                                                 |
| CDN                        | [CDN](#cdn)                                                 |
| Cold start                 | [Cold Start](#cold-start)                                   |
| DAX                        | [DAX](#dax)                                                 |
| DLQ                        | [DLQ](#dlq)                                                 |
| DMS / SCT                  | [DMS](#dms) · [SCT](#sct)                                   |
| DNS                        | [DNS](#dns)                                                 |
| EBS / EFS / FSx            | [EBS](#ebs) · [EFS](#efs) · [FSx](#fsx)                     |
| EC2 / ECS / EKS / ECR      | [EC2](#ec2) · [ECS](#ecs) · [EKS](#eks) · [ECR](#ecr)       |
| ENI                        | [ENI](#eni)                                                 |
| ETL                        | [ETL](#etl)                                                 |
| Fan-out                    | [Fan-Out](#fan-out)                                         |
| FIPS                       | [FIPS](#fips)                                               |
| HA                         | [HA](#ha--high-availability)                                |
| HSM                        | [HSM](#hsm)                                                 |
| IAM                        | [IAM](#iam)                                                 |
| IGW / NAT                  | [IGW](#igw) · [NAT](#nat)                                   |
| IOPS                       | [IOPS](#iops)                                               |
| KMS                        | [KMS](#kms)                                                 |
| LSI / GSI                  | [LSI](#lsi) · [GSI](#gsi)                                   |
| MFA / SSO / STS            | [MFA](#mfa) · [SSO](#sso) · [STS](#sts)                     |
| Multi-AZ                   | [Multi-AZ](#multi-az)                                       |
| Multi-Region               | [Multi-Region](#multi-region)                               |
| NFS / SMB                  | [NFS](#nfs) · [SMB](#smb)                                   |
| OAC / OAI                  | [OAC/OAI](#oac--oai)                                        |
| OLAP / OLTP                | [OLAP](#olap) · [OLTP](#oltp)                               |
| Pilot light / Warm standby | [Pilot Light](#pilot-light) · [Warm Standby](#warm-standby) |
| POSIX                      | [POSIX](#posix)                                             |
| Provisioned Concurrency    | [Provisioned Concurrency](#provisioned-concurrency)         |
| RDS                        | [RDS](#rds)                                                 |
| RPO / RTO                  | [RPO](#rpo) · [RTO](#rto)                                   |
| S3                         | [S3](#s3)                                                   |
| SCP                        | [SCP](#scp)                                                 |
| Serverless                 | [Serverless](#serverless)                                   |
| SNS / SQS                  | [SNS](#sns) · [SQS](#sqs)                                   |
| Stateful / Stateless       | [Stateful vs Stateless](#stateful-vs-stateless)             |
| TLS / SSL                  | [TLS/SSL](#tls--ssl)                                        |
| Visibility Timeout         | [Visibility Timeout](#visibility-timeout)                   |
| VPC                        | [VPC](#vpc)                                                 |
| WAF                        | [WAF](#waf)                                                 |

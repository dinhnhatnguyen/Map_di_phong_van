# Question Patterns And Keywords

SAA-C03 hay dùng scenario dài nhưng core decision thường chỉ xoay quanh 1-2 constraint. Học nhận pattern nhanh để không mất thời gian đọc lại câu.

## Cách Đọc Câu Hỏi

1. **Gạch requirement chính**: thường là một trong các nhóm sau:
   - Cost: "cost-effective", "minimize cost", "least expensive"
   - Availability/Resilience: "highly available", "fault tolerant", "no single point of failure"
   - Security: "least privilege", "encrypt", "private", "compliance"
   - Performance: "low latency", "high throughput", "fast", "scalable"
   - Operational: "least operational overhead", "managed", "no server management"

2. **Xác định constraint phụ**: thường là điều kiện loại đáp án sai, ví dụ "on-premises", "existing VPN", "without code changes".

3. **Loại đáp án rõ sai trước**: single AZ cho HA, public subnet cho DB, manual process khi đề yêu cầu managed.

---

## Pattern 1: High Availability / Fault Tolerance

**Từ khóa**: highly available, fault tolerant, no single point of failure, survive AZ failure

**Bẫy**:
- Multi-AZ instance tốt nhưng nếu không có ASG thì vẫn single point nếu instance fail
- RDS Multi-AZ chỉ trong một Region; không thay Multi-Region DR
- Single AZ = single point of failure cho hầu hết resource

**Rule**:
- Web app HA → ALB + ASG multi-AZ
- DB HA → RDS Multi-AZ hoặc Aurora (Aurora tự replicate 6 copies 3 AZ)
- Storage HA → S3 (multi-AZ by default), EFS Standard (multi-AZ), tránh EBS/EFS One Zone cho production critical

---

## Pattern 2: Least Operational Overhead

**Từ khóa**: least operational overhead, minimize management, no server management, fully managed

**Bẫy**:
- Elastic Beanstalk vẫn cần quản lý (managed platform nhưng không serverless)
- ECS on EC2 cần quản lý EC2 fleet
- Self-managed database trên EC2 tốn overhead nhất

**Rule** (từ ít overhead đến nhiều):
- Lambda > Fargate > ECS on EC2 > EC2 tự cài app
- DynamoDB/Aurora Serverless > RDS > DB trên EC2
- S3/SQS/SNS/EventBridge > tự quản service

---

## Pattern 3: Cost Optimization

**Từ khóa**: most cost-effective, minimize cost, reduce cost, cheapest

**Bẫy**:
- Spot Instances rẻ nhưng không dùng cho workload không chịu interrupt (production web app, DB)
- NAT Gateway có phí data processing; thay bằng Gateway endpoint cho S3/DynamoDB nếu đó là traffic chính
- Multi-AZ tốn gấp đôi; chỉ dùng khi HA thật sự cần

**Rule**:
- Workload interruptible, batch → Spot
- Stable baseline → Savings Plans (compute) hoặc RI (DB/ElastiCache)
- Archive dữ liệu → S3 Glacier (Instant/Flexible/Deep Archive tùy retrieval)
- Private subnet gọi S3/DynamoDB → Gateway endpoint (không tính hourly, tiết kiệm NAT)

---

## Pattern 4: Security / Least Privilege

**Từ khóa**: secure, least privilege, encrypt, private, compliance, audit

**Bẫy**:
- Tạo IAM user với long-term access key cho EC2/Lambda là sai; dùng IAM role
- Public bucket S3 cho nội dung private là sai
- KMS với customer managed key khi đề nói cần control/audit key
- CloudHSM khi đề nói dedicated HSM hoặc FIPS 140-2 Level 3

**Rule**:
- EC2/Lambda gọi AWS service → IAM role (instance profile/execution role)
- Cross-account → trust policy + assume role
- Human access nhiều account → IAM Identity Center
- Encrypt at rest với audit → KMS CMK
- DB credentials rotation → Secrets Manager
- Config parameter không secret → Parameter Store (rẻ hơn Secrets Manager)

---

## Pattern 5: Decoupling / Loose Coupling

**Từ khóa**: decouple, loosely coupled, asynchronous, buffer, queue, event-driven

**Bẫy**:
- Lambda gọi Lambda trực tiếp = tight coupling; dùng SQS/EventBridge ở giữa
- Direct HTTP call giữa service = tight coupling
- SNS không lưu message; SQS lưu đến retention period

**Rule**:
- Buffer traffic spike → SQS
- Fan-out một event → SNS → SQS (nhiều consumer)
- Event routing phức tạp / SaaS events → EventBridge
- Multi-step workflow / retry / branch → Step Functions
- Realtime stream → Kinesis Data Streams

---

## Pattern 6: Disaster Recovery

**Từ khóa**: disaster recovery, RTO, RPO, region failure, data loss tolerance

**Bẫy**:
- Multi-AZ trong cùng Region không bảo vệ khỏi Region failure
- Pilot light cần thời gian "warm up" compute trước khi service; không phải zero-downtime
- Backup and restore có RTO/RPO cao nhất (giờ đến ngày)

**DR Strategy chọn**:
- Chấp nhận downtime cao, budget thấp → Backup and Restore
- Chấp nhận downtime trung bình → Pilot Light (core data luôn chạy)
- Downtime thấp → Warm Standby (reduced capacity ready)
- Near-zero downtime → Active-Active (Global Tables, Aurora Global, Multi-Region ALB/Route 53)

---

## Pattern 7: Data Lake / Analytics

**Từ khóa**: analyze logs, query data, data lake, stream ingestion, ETL, BI dashboard

**Rule**:
- Data vào S3 → query SQL ad-hoc → **Athena**
- Data warehouse repeated analytics → **Redshift**
- ETL transform / catalog metadata → **Glue**
- Realtime stream ingest → **Kinesis Data Streams**
- Stream deliver to S3/Redshift/OpenSearch → **Kinesis Firehose** (Amazon Data Firehose)
- Dashboard → **QuickSight**
- Log search/analytics → **OpenSearch**
- Kafka workload → **MSK**

---

## Pattern 8: Migration

**Từ khóa**: migrate to AWS, minimal downtime, on-premises, lift-and-shift

**Rule**:
- Database, minimal downtime → **DMS với CDC (ongoing replication)**
- Heterogeneous DB (Oracle → Aurora) → **SCT + DMS**
- File/object migrate online → **DataSync**
- On-prem app vẫn dùng NFS/SMB nhưng data lên S3 → **Storage Gateway File Gateway**
- Offline large data (100TB+, slow internet) → **Snow Family**
- SFTP partner upload → **Transfer Family**
- Lift-and-shift VM/server → **Application Migration Service**

---

## Pattern 9: Networking Decision

**Từ khóa**: private subnet, access AWS services, VPC, connectivity, on-premises

**Rule**:
- Private subnet gọi S3/DynamoDB → Gateway endpoint (route table, free hourly)
- Private subnet gọi Secrets Manager/CloudWatch/KMS/etc → Interface endpoint (ENI, có hourly)
- Nhiều VPC kết nối → Transit Gateway
- 2 VPC đơn giản → VPC Peering (chú ý không transitive)
- Expose service privately → PrivateLink
- On-prem dedicated → Direct Connect
- On-prem encrypted nhanh → Site-to-Site VPN
- On-prem dedicated + encrypted → Direct Connect + VPN

---

## Pattern 10: Storage Selection

**Từ khóa**: store files, shared storage, block storage, archive

**Rule**:
- Object/file/static/log/backup → S3
- Shared Linux file system → EFS
- Windows SMB / Active Directory → FSx for Windows
- HPC / ML high throughput → FSx for Lustre
- EC2 boot disk / database volume → EBS
- Archive 7+ years compliance → S3 Glacier Deep Archive
- Unknown access pattern → S3 Intelligent-Tiering
- Centralized backup → AWS Backup

---

## Câu Hỏi Kiểm Tra Nhanh Trước Khi Chọn Đáp Án

1. Đáp án này có vi phạm HA requirement không? (single AZ, single instance)
2. Đáp án này có để resource public khi đề yêu cầu private/secure không?
3. Đáp án này có overhead cao hơn một đáp án khác cũng đúng không?
4. Đáp án này có phù hợp với scale của đề nói không? (Lambda cho job 20 phút = sai)
5. Đáp án này có tốn cost cao hơn không cần thiết không?

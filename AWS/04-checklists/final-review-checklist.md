# Final Review Checklist

Đọc danh sách này trong 24-48 giờ trước khi thi. Mỗi mục là một điểm hay bị bẫy.

## Security (30%)

### IAM
- [ ] Explicit deny thắng mọi allow
- [ ] SCP không cấp quyền, chỉ giới hạn maximum permission
- [ ] SCP không ảnh hưởng service-linked roles
- [ ] Permission boundary giới hạn maximum, không tự cấp quyền
- [ ] EC2/Lambda/ECS dùng IAM role, không dùng access key hardcode
- [ ] Cross-account: account B tạo role trust account A; account A assume role qua STS

### Encryption
- [ ] SSE-KMS cần quyền S3 + quyền KMS key để decrypt cross-account
- [ ] Force encrypt upload S3: bucket policy deny nếu thiếu `x-amz-server-side-encryption` header
- [ ] ACM cert cho CloudFront phải tạo ở **us-east-1**
- [ ] KMS = FIPS 140-2 Level 2; CloudHSM = FIPS 140-2 Level 3 (dedicated hardware)
- [ ] Secrets Manager tự động rotate; Parameter Store SecureString dùng KMS nhưng không rotate tự động

### Network Security
- [ ] Security Group stateful, allow only, default deny all inbound
- [ ] NACL stateless, có deny, đánh giá theo rule number thứ tự tăng dần
- [ ] Không đặt database ở public subnet
- [ ] DB SG chỉ allow từ App SG, không mở CIDR rộng

---

## Resilience (26%)

### High Availability
- [ ] Multi-AZ != Multi-Region; Multi-AZ cho HA trong Region, Multi-Region cho DR
- [ ] RDS Multi-AZ = HA/failover (synchronous standby, không scale đọc)
- [ ] RDS Read Replica = scale đọc (asynchronous, có thể promote khi DR)
- [ ] Aurora storage replicate 6 copies across 3 AZ
- [ ] ASG multi-AZ sau ALB = HA cho stateless app
- [ ] Session state externalize vào ElastiCache/DynamoDB khi scale horizontal

### DR Strategies
- [ ] Backup and restore: thấp nhất cost, cao nhất RTO/RPO
- [ ] Pilot light: chỉ data layer always running, compute tắt
- [ ] Warm standby: reduced-capacity environment running
- [ ] Active-active: lowest RTO/RPO, cao nhất cost
- [ ] Route 53 Failover routing + health check cho active-passive DR

### Loose Coupling
- [ ] SQS buffer traffic spike và decouple producer/consumer
- [ ] SNS fan-out một event ra nhiều consumer
- [ ] EventBridge rule-based routing, SaaS/AWS/custom events
- [ ] Step Functions orchestrate nhiều bước có retry/branch

---

## Performance (24%)

### Compute
- [ ] Lambda: timeout max 15 phút; memory 128MB–10,240MB; tăng memory = tăng CPU
- [ ] Lambda cold start: giảm bằng Provisioned Concurrency
- [ ] Lambda trong VPC cần NAT hoặc endpoint để ra ngoài VPC
- [ ] Fargate: không quản lý EC2, giảm operational overhead
- [ ] ASG scale theo queue depth: CloudWatch metric từ SQS ApproximateNumberOfMessages

### Storage Performance
- [ ] EBS gp3: cấu hình IOPS/throughput độc lập, default tốt
- [ ] EBS io2/io2 Block Express: mission-critical, IOPS cao nhất, latency thấp
- [ ] S3 multipart upload cho object lớn (bắt buộc >5GB, khuyến nghị >100MB)
- [ ] S3 Transfer Acceleration cho upload/download xa Region
- [ ] CloudFront giảm latency và origin load cho static/dynamic content

### Database Performance
- [ ] DAX: in-memory cache cho DynamoDB, microsecond read, không giúp write-heavy
- [ ] ElastiCache Redis: cache DB read-heavy, session, leaderboard
- [ ] Aurora reader endpoint cho read scaling
- [ ] RDS Proxy: connection pooling cho Lambda/serverless gọi RDS
- [ ] DynamoDB On-Demand cho unpredictable traffic; Provisioned + autoscaling cho predictable

### Networking Performance
- [ ] CloudFront: HTTP caching, signed URL, WAF, OAC cho private S3
- [ ] Global Accelerator: static anycast IP, TCP/UDP acceleration, fast regional failover không phụ thuộc DNS TTL
- [ ] NLB: static IP, TCP/UDP/TLS, ultra-low latency, high throughput
- [ ] ALB: HTTP/HTTPS path/host-based routing, WebSocket, Lambda target

---

## Cost Optimization (20%)

### Compute Cost
- [ ] Spot: fault-tolerant, interruptible workload (batch/worker); có thể bị reclaim
- [ ] Savings Plans: cam kết spend/giờ, linh hoạt instance type/region/size (EC2 + Fargate + Lambda)
- [ ] Reserved Instances: cam kết instance type cụ thể; RI riêng cho RDS/ElastiCache/Redshift
- [ ] On-Demand: flexible, không cam kết, đắt nhất
- [ ] Dedicated Host: per-host license, BYOL, compliance isolation

### Storage Cost
- [ ] S3 Standard-IA và One Zone-IA: minimum 30 ngày, retrieval fee
- [ ] S3 Glacier Instant: minimum 90 ngày; Glacier Flexible: 90 ngày; Deep Archive: 180 ngày
- [ ] S3 Intelligent-Tiering: object < 128KB không tiết kiệm được vì không tự chuyển tier
- [ ] Lifecycle transition: tính phí tối thiểu dù object bị xóa/chuyển trước hạn
- [ ] Gateway VPC endpoint cho S3/DynamoDB: không tính hourly, giảm NAT cost

### Network Cost
- [ ] NAT Gateway: phí hourly + data processing/GB; thay bằng Gateway endpoint cho S3/DynamoDB
- [ ] Cross-AZ data transfer tốn phí; đặt NAT Gateway mỗi AZ nếu cần HA nhưng nhớ nhân đôi cost
- [ ] CloudFront giảm data transfer từ origin (S3/ALB) ra internet

---

## Số Liệu Quan Trọng Cần Nhớ

| Item | Giá trị |
|---|---|
| Lambda timeout max | 15 phút |
| Lambda memory | 128MB – 10,240MB |
| SQS visibility timeout default | 30 giây |
| SQS visibility timeout max | 12 giờ |
| DynamoDB item size max | 400KB |
| S3 single PUT max | 5GB |
| S3 multipart upload max | 5TB |
| S3 Standard-IA min duration | 30 ngày |
| S3 Glacier Instant/Flexible min | 90 ngày |
| S3 Glacier Deep Archive min | 180 ngày |
| S3 Intelligent-Tiering min object | 128KB (nhỏ hơn không tự tier) |
| Aurora storage copies | 6 bản trong 3 AZ |
| Exam questions | 65 câu (50 scored + 15 unscored) |
| Exam duration | 130 phút |
| Passing score | 720/1000 |

---

## Từ Khóa → Dịch Vụ Nhanh

| Từ khóa trong đề | Nghĩ tới |
|---|---|
| least operational overhead | serverless / managed / AWS Backup / lifecycle |
| decouple | SQS, SNS, EventBridge |
| fan-out | SNS → SQS |
| exactly-once / FIFO | SQS FIFO |
| shared Linux file system | EFS |
| Windows SMB / Active Directory | FSx for Windows |
| high IOPS database | EBS io2 |
| private access to S3/DynamoDB | Gateway VPC endpoint |
| private access to AWS services | Interface endpoint / PrivateLink |
| database HA / failover | RDS Multi-AZ hoặc Aurora |
| read scaling database | Read Replica, Aurora reader endpoint |
| global active-active NoSQL | DynamoDB Global Tables |
| immutable retention | S3 Object Lock Compliance mode |
| centralized governance multi-account | Organizations + SCP + Control Tower |
| dedicated HSM / FIPS Level 3 | CloudHSM |
| DDoS protection advanced | Shield Advanced |
| detect SQL injection / XSS | WAF |
| S3 sensitive data | Macie |
| threat detection | GuardDuty |
| vulnerability scan | Inspector |
| who did what API audit | CloudTrail |
| resource compliance history | AWS Config |
| distributed tracing | X-Ray |
| encrypted dedicated on-prem link | Direct Connect + VPN |
| fast encrypted on-prem quick deploy | Site-to-Site VPN |
| offline huge data transfer TB-PB | Snow Family |
| migrate database minimal downtime | DMS CDC |
| convert schema heterogeneous DB | SCT + DMS |

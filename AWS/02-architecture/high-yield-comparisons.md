# High-Yield Comparisons

Những cặp dưới đây là nơi đề SAA-C03 hay gài. Hãy học bằng cách tự giải thích "tại sao không chọn cái còn lại".

## RDS Multi-AZ vs Read Replica

| Multi-AZ | Read Replica |
|---|---|
| Mục tiêu: HA/failover | Mục tiêu: scale đọc |
| Thường synchronous standby | Asynchronous replication |
| Failover tự động | Có thể promote nhưng không phải failover chính |
| Không dùng để tăng read trong pattern cổ điển | App đọc từ replica |

Rule: cần availability/failover -> Multi-AZ. Cần giảm tải đọc -> Read Replica.

## SQS vs SNS vs EventBridge vs Step Functions

| Service | Mental model | Chọn khi |
|---|---|---|
| SQS | Queue | Worker pull job, decouple producer/consumer |
| SNS | Broadcast | Fan-out push notification |
| EventBridge | Event router | Match rule, SaaS/AWS/custom event bus |
| Step Functions | Workflow brain | Orchestrate nhiều bước/retry/branch |

Rule: queue = SQS, fan-out = SNS, event routing = EventBridge, workflow = Step Functions.

## ALB vs NLB vs GWLB

| ALB | NLB | GWLB |
|---|---|---|
| Layer 7 HTTP/HTTPS | Layer 4 TCP/UDP/TLS | Appliance insertion |
| Host/path/header routing | Static IP, extreme performance | Firewall/IDS/IPS |
| Web/container/Lambda target | Non-HTTP/low latency | Transparent inspection |

## NAT Gateway vs VPC Endpoint

| NAT Gateway | VPC Endpoint |
|---|---|
| Private subnet ra internet outbound | Private access tới AWS service |
| Có phí hourly/data | Gateway endpoint cho S3/DDB thường tiết kiệm hơn |
| Dùng cho update OS/download packages | Dùng cho S3, DynamoDB, Secrets Manager, CloudWatch... |

Rule: gọi AWS service privately -> endpoint. Ra internet nói chung -> NAT.

## Gateway Endpoint vs Interface Endpoint

| Gateway Endpoint | Interface Endpoint |
|---|---|
| S3, DynamoDB | Hầu hết AWS services |
| Route table target | ENI private IP |
| Không có SG riêng | Có Security Group |
| Không hourly charge như interface | Có hourly + data processing |

## CloudFront vs Global Accelerator

| CloudFront | Global Accelerator |
|---|---|
| CDN/cache HTTP content | Network acceleration TCP/UDP |
| Edge cache, signed URL/cookie | Static anycast IP |
| Origin S3/ALB/API/custom | Endpoint ALB/NLB/EC2 EIP |
| Tối ưu web content | Tối ưu routing/failover network |

## S3 vs EBS vs EFS vs FSx

| Service | Type | Chọn khi |
|---|---|---|
| S3 | Object | files, data lake, static, backup |
| EBS | Block | disk cho một EC2 trong AZ |
| EFS | File NFS | shared Linux POSIX file system |
| FSx | Managed file | Windows SMB, Lustre HPC, ONTAP/OpenZFS |

## S3 Lifecycle vs Intelligent-Tiering

| Lifecycle | Intelligent-Tiering |
|---|---|
| Bạn biết tuổi dữ liệu/access pattern | Access pattern không rõ/thay đổi |
| Rule explicit chuyển class/xóa | AWS tự move giữa tiers |
| Tốt cho logs/archive predictable | Tốt cho user-generated/data lake unknown |

## EBS gp3 vs io2 vs st1/sc1

| Type | Chọn khi |
|---|---|
| gp3 | Default SSD cân bằng, app/boot/general DB |
| io2 | Mission-critical high IOPS/low latency |
| st1 | Sequential throughput big data/log |
| sc1 | Cold HDD low-cost |

## EFS Standard vs EFS One Zone

| Standard | One Zone |
|---|---|
| Multi-AZ regional | Một AZ |
| HA hơn | Rẻ hơn |
| Production shared FS | Dev/test hoặc data có thể recreate |

## DynamoDB On-Demand vs Provisioned

| On-Demand | Provisioned |
|---|---|
| Unknown/spiky traffic | Predictable traffic |
| Ít quản lý | Có thể rẻ hơn nếu ổn định |
| Pay per request | RCU/WCU + autoscaling/reserved |

## DynamoDB GSI vs LSI

| GSI | LSI |
|---|---|
| Partition key khác được | Cùng partition key với table |
| Tạo sau cũng được | Chỉ tạo lúc tạo table |
| Eventual consistency | Có thể strongly consistent |

## DAX vs ElastiCache

| DAX | ElastiCache |
|---|---|
| Cache riêng cho DynamoDB API-compatible | General-purpose in-memory cache |
| Read-heavy DynamoDB | RDS/session/custom cache |
| Microsecond read for DDB | Redis/Valkey/Memcached features |

## Athena vs Redshift vs EMR

| Athena | Redshift | EMR |
|---|---|---|
| Serverless SQL on S3 | Data warehouse | Hadoop/Spark cluster |
| Ad-hoc query | Repeated BI/OLAP | Big data processing control |
| Pay per scanned data | Provisioned/serverless warehouse | Cluster/job oriented |

## DMS vs DataSync vs Storage Gateway

| DMS | DataSync | Storage Gateway |
|---|---|---|
| Database migration | File/object transfer | Hybrid local interface |
| CDC/minimal downtime | NFS/SMB/S3/EFS/FSx sync | File/Volume/Tape gateway |
| DB engines | Data copy/sync | On-prem app keeps using local protocol |

## KMS vs Secrets Manager vs Parameter Store

| KMS | Secrets Manager | Parameter Store |
|---|---|---|
| Key management/encryption | Store/rotate secrets | Config/parameters/secrets |
| Encrypt/decrypt data keys | DB password/API key rotation | Simpler/lower cost config store |
| Used by S3/EBS/RDS/etc. | Rotation workflows | SecureString can use KMS |

## IAM Role vs Resource Policy

| IAM Role | Resource Policy |
|---|---|
| Who can do what | Who can access this resource |
| Assumed by principal/service | Attached to resource |
| EC2/Lambda/cross-account temporary access | S3 bucket/SQS/SNS/KMS/Lambda invoke permissions |

## Route 53 Latency vs Geolocation vs Failover vs Weighted

| Policy | Chọn khi |
|---|---|
| Latency | Route tới Region latency thấp nhất |
| Geolocation | Route theo quốc gia/vị trí user |
| Failover | Active-passive health check |
| Weighted | Chia phần trăm traffic |

## Savings Plans vs Reserved Instances

| Savings Plans | Reserved Instances |
|---|---|
| Cam kết spend ($/giờ) cho compute | Cam kết instance type/family/Region cụ thể |
| Compute SP: EC2 + Fargate + Lambda | Standard RI: một instance type/size cố định |
| EC2 Instance SP: linh hoạt hơn Standard RI | Convertible RI: đổi được type/OS/tenancy |
| Linh hoạt hơn RI khi cần đổi instance type/region | Rẻ hơn một chút nếu workload hoàn toàn ổn định |
| Không áp dụng cho RDS/ElastiCache/Redshift | RI riêng cho RDS, ElastiCache, Redshift |

Rule: workload compute có thể đổi instance type/size/region -> Savings Plans. Muốn discount cho RDS/ElastiCache -> Reserved Instances riêng cho từng service.

## Backup And DR

| Strategy | Cost | RTO/RPO |
|---|---:|---:|
| Backup and restore | Thấp | Cao |
| Pilot light | Thấp-vừa | Trung bình |
| Warm standby | Vừa-cao | Thấp |
| Active-active | Cao | Rất thấp |

Rule: đề nhấn thấp cost và chấp nhận downtime -> backup/restore. Đề nhấn near-zero downtime -> warm standby/active-active.

# Learning Roadmap

Roadmap này ưu tiên học để làm được câu scenario. Mỗi ngày nên học 60-120 phút: đọc note, vẽ lại kiến trúc, làm 10-20 câu, ghi lỗi.

## Lộ Trình 21 Ngày

| Ngày | Chủ đề | Việc cần làm |
|---:|---|---|
| 1 | Blueprint + Well-Architected | Đọc [SAA-C03 Blueprint](SAA-C03-blueprint.md), nhớ 4 domain và 6 pillar |
| 2 | IAM + account governance | IAM user/group/role/policy, STS, Organizations, SCP, IAM Identity Center |
| 3 | KMS + data security | encryption at rest/in transit, KMS key policy, Secrets Manager, ACM, S3 security |
| 4 | VPC fundamentals | CIDR, subnet, route table, IGW, NAT Gateway, SG vs NACL |
| 5 | Network design | VPC endpoint, PrivateLink, Transit Gateway, VPN, Direct Connect, Route 53 |
| 6 | S3 | storage class, lifecycle, versioning, replication, Object Lock, static hosting, CloudFront |
| 7 | EBS/EFS/FSx/Backup | block vs file, snapshot, Multi-AZ file system, backup strategy |
| 8 | EC2 + Auto Scaling | pricing options, instance family, ASG policy, ALB/NLB/GWLB |
| 9 | Lambda + API Gateway | serverless pattern, concurrency, event source, VPC Lambda, timeout |
| 10 | Containers | ECS, Fargate, EKS, ECR, when to use container vs Lambda vs EC2 |
| 11 | RDS + Aurora | Multi-AZ, read replica, RDS Proxy, Aurora endpoints, backup/PITR |
| 12 | DynamoDB | partition key, sort key, GSI/LSI, on-demand/provisioned, DAX, Streams, Global Tables |
| 13 | Caching + purpose-built DB | ElastiCache, Redshift, OpenSearch, DocumentDB, Neptune, Keyspaces |
| 14 | Integration | SQS, SNS, EventBridge, Step Functions, AppSync, API Gateway |
| 15 | Observability + governance | CloudWatch, CloudTrail, Config, Systems Manager, Trusted Advisor, Compute Optimizer |
| 16 | Migration + hybrid | DMS, DataSync, Storage Gateway, Snow Family, Transfer Family, Application Migration Service |
| 17 | Analytics | Athena, Glue, Kinesis, EMR, Lake Formation, QuickSight, Redshift |
| 18 | Architecture patterns | Đọc [Architecture Patterns](../02-architecture/architecture-patterns.md), vẽ lại bằng tay |
| 19 | Decision matrix | Đọc [Service Selection Matrix](../02-architecture/service-selection-matrix.md) và [High-Yield Comparisons](../02-architecture/high-yield-comparisons.md) |
| 20 | Mock 1 | Làm [Mock Exam 01](../03-exam-practice/mock-exam-01.md), ghi lỗi |
| 21 | Mock 2 + final review | Làm [Mock Exam 02](../03-exam-practice/mock-exam-02.md), ôn [Final Checklist](../04-checklists/final-review-checklist.md) |

## Lộ Trình 30 Ngày

Nếu có nhiều thời gian hơn, dùng 21 ngày đầu để học hết nội dung, 9 ngày sau để luyện đề:

| Ngày | Chủ đề |
|---:|---|
| 22 | Làm lại câu sai domain Security |
| 23 | Làm lại câu sai domain Resilience |
| 24 | Làm lại câu sai domain Performance |
| 25 | Làm lại câu sai domain Cost |
| 26 | Vẽ 10 architecture pattern không nhìn tài liệu |
| 27 | Ôn keyword traps và high-yield comparisons |
| 28 | Làm mock theo thời gian thật: 65 câu / 130 phút |
| 29 | Ôn flashcards, service limits quan trọng, storage class |
| 30 | Nghỉ nhẹ, đọc final checklist, ngủ đủ |

## Cách Ghi Chép Để Nhớ Lâu

Mỗi service nên viết theo mẫu:

```text
Service:
Là gì:
Dùng khi:
Không dùng khi:
HA/DR:
Security:
Performance:
Cost:
Bẫy đề:
```

## Nhịp Luyện Đề

- Lần 1: làm chậm, đọc giải thích.
- Lần 2: làm lại chỉ câu sai sau 24 giờ.
- Lần 3: làm full timed.
- Sau mỗi mock, phân loại lỗi vào [Wrong Answer Log](../04-checklists/wrong-answer-log.md).

## Điểm Đạt Mục Tiêu Trước Khi Thi

- Mock ổn định trên 80%.
- Giải thích được vì sao 3 đáp án sai bị loại.
- Vẽ được 3-tier, serverless, event-driven, DR warm standby, hybrid storage.
- Phân biệt nhanh: RDS Multi-AZ vs Read Replica, ALB vs NLB, SQS vs SNS vs EventBridge, NAT Gateway vs VPC Endpoint, EFS vs EBS vs S3.

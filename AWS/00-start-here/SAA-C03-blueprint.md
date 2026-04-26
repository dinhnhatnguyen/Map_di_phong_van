# SAA-C03 Blueprint

File này là bản đồ học. Mục tiêu là biết đề thi đang kiểm tra năng lực thiết kế kiến trúc, không kiểm tra nhớ lệnh CLI hay lập trình sâu.

## Exam Overview

Theo trang AWS Certification chính thức:

- Tên chứng chỉ: AWS Certified Solutions Architect - Associate.
- Mã đề: SAA-C03.
- Thời lượng: 130 phút.
- Format: 65 câu, gồm multiple choice và multiple response.
- Kết quả: scaled score 100-1000, điểm đạt tối thiểu 720.
- AWS Exam Guide nói trong 65 câu có 50 câu tính điểm và 15 câu không tính điểm, nhưng thí sinh không biết câu nào là unscored.

Nguồn: [AWS certification page](https://aws.amazon.com/certification/certified-solutions-architect-associate/) và [AWS SAA-C03 Exam Guide PDF](https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf).

## 4 Domain Chính

| Domain | Trọng số | Năng lực cần có | File học chính |
|---|---:|---|---|
| Design Secure Architectures | 30% | IAM, network security, data protection, encryption, account governance | [Security](../01-core-services/security-identity.md), [Networking](../01-core-services/networking.md), [Storage](../01-core-services/storage.md) |
| Design Resilient Architectures | 26% | loose coupling, HA, fault tolerance, DR, scaling | [Architecture Patterns](../02-architecture/architecture-patterns.md), [Integration](../01-core-services/integration-observability-cost.md), [Databases](../01-core-services/databases.md) |
| Design High-Performing Architectures | 24% | storage, compute, database, network, ingestion performance | [Compute](../01-core-services/compute.md), [Databases](../01-core-services/databases.md), [Migration Analytics Edge](../01-core-services/migration-analytics-edge.md) |
| Design Cost-Optimized Architectures | 20% | right sizing, pricing model, lifecycle, data transfer, managed services | [Service Matrix](../02-architecture/service-selection-matrix.md), [Cost](../01-core-services/integration-observability-cost.md) |

## Những Chủ Đề Phải Nắm

AWS Exam Guide liệt kê các nhóm concept có thể xuất hiện:

- Compute
- Cost management
- Database
- Disaster recovery
- High performance
- Management and governance
- Microservices and component delivery
- Migration and data transfer
- Networking, connectivity, and content delivery
- Resiliency
- Security
- Serverless and event-driven design principles
- Storage

## In-Scope Services Theo Nhóm

Đây là danh sách trọng tâm được rút gọn từ Exam Guide. Không phải service nào cũng xuất hiện nhiều như nhau; hãy ưu tiên core services trước.

### Rất Quan Trọng

- IAM, IAM Identity Center, Organizations, Control Tower, SCP
- VPC, subnet, route table, security group, network ACL, NAT Gateway, VPC endpoints
- ELB: ALB, NLB, Gateway Load Balancer
- Route 53, CloudFront, Global Accelerator, Direct Connect, Site-to-Site VPN, Transit Gateway, PrivateLink
- EC2, Auto Scaling, Launch Template, placement group, Spot, Reserved Instances, Savings Plans
- Lambda, Fargate, ECS, EKS, ECR
- S3, S3 Glacier, EBS, EFS, FSx, AWS Backup, Storage Gateway
- RDS, Aurora, DynamoDB, ElastiCache, Redshift
- SQS, SNS, EventBridge, Step Functions, API Gateway, AppSync
- CloudWatch, CloudTrail, Config, Systems Manager, Trusted Advisor, Compute Optimizer
- KMS, ACM, Secrets Manager, WAF, Shield, Cognito, GuardDuty, Macie, Inspector, Security Hub
- DataSync, DMS, Snow Family, Transfer Family

### Nên Biết Để Không Mất Điểm

- Athena, Glue, Kinesis, EMR, Lake Formation, QuickSight, OpenSearch, MSK
- DocumentDB, Neptune, Keyspaces, QLDB
- AppFlow, Amazon MQ
- Elastic Beanstalk, Batch, Outposts, Wavelength, VMware Cloud on AWS
- CloudFormation, Service Catalog, Well-Architected Tool
- Comprehend, Polly, Rekognition, Textract, Transcribe, Translate, SageMaker ở mức use case

### Out-of-Scope Cần Nhận Ra

Một số service được AWS liệt kê là ngoài phạm vi SAA-C03, ví dụ Lightsail, Cloud9, CDK, CodeBuild, CodeCommit, CodeDeploy, IoT services, GameLift, Ground Station. Nếu xuất hiện trong đáp án practice bên ngoài, kiểm tra lại vì thường không phải lựa chọn đúng cho SAA-C03.

## Cách Đề Hỏi

### Multiple Choice

Một đáp án đúng. Cách xử lý:

1. Gạch requirement chính: cost, security, latency, HA, least operational overhead.
2. Loại đáp án sai rõ: single AZ cho HA, public subnet cho database, manual process khi đề yêu cầu managed.
3. So đáp án còn lại bằng trade-off.

### Multiple Response

Hai hoặc nhiều đáp án đúng. Cách xử lý:

- Đếm số đáp án đề yêu cầu chọn nếu có ghi "Choose TWO" hoặc "Choose THREE".
- Mỗi đáp án phải thỏa requirement độc lập hoặc là một phần bắt buộc trong kiến trúc.
- Cẩn thận đáp án "đúng về mặt kỹ thuật" nhưng không tối ưu theo constraint.

## Map Năng Lực Với Câu Hỏi

| Nếu đề nói... | Nghĩ tới... |
|---|---|
| "least operational overhead" | managed service, serverless, automated scaling, AWS Backup, lifecycle |
| "decouple components" | SQS, SNS, EventBridge, Step Functions |
| "fan-out to multiple consumers" | SNS topic to SQS queues |
| "exactly once / ordered processing" | SQS FIFO hoặc Kinesis shard ordering tùy context |
| "shared file system for Linux instances" | EFS |
| "Windows SMB file share" | FSx for Windows File Server |
| "high IOPS block storage" | EBS io2 Block Express hoặc gp3 tùy yêu cầu |
| "static website global low latency" | S3 + CloudFront + OAC/OAI |
| "private access to S3/DynamoDB from VPC" | Gateway VPC endpoint |
| "private access to most AWS services/partner services" | Interface endpoint / PrivateLink |
| "database failover" | RDS Multi-AZ hoặc Aurora |
| "database read scaling" | Read Replica, Aurora reader endpoint, ElastiCache |
| "global active-active NoSQL" | DynamoDB Global Tables |
| "SQL compatible high performance" | Aurora MySQL/PostgreSQL |
| "immutable object retention" | S3 Object Lock |
| "centralized multi-account governance" | AWS Organizations, Control Tower, SCP |

## Nguồn Chính

- [AWS SAA-C03 Exam Guide PDF](https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf)
- [AWS Certified Solutions Architect - Associate](https://aws.amazon.com/certification/certified-solutions-architect-associate/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

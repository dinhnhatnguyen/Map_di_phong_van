# SAA-C03 Blueprint

> Gặp từ viết tắt không quen? Xem **[Từ Điển](glossary.md)**.

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

> Nhấp vào tên dịch vụ để xem giải thích chi tiết trong **[Services Overview](services-overview.md)**.

AWS Exam Guide liệt kê các nhóm concept có thể xuất hiện:

- Compute _(máy tính/xử lý: EC2, Lambda, container)_
- Cost management _(quản lý chi phí: tối ưu, giám sát, dự báo ngân sách)_
- Database _(cơ sở dữ liệu: SQL, NoSQL, cache, warehouse)_
- Disaster recovery _(khôi phục sau thảm họa: backup, failover, [RTO/RPO](glossary.md#rto))_
- High performance _(hiệu năng cao: IOPS, throughput, caching, CDN)_
- Management and governance _(quản trị: audit, compliance, IaC, patch)_
- Microservices and component delivery _(kiến trúc vi dịch vụ: container, API, decoupling)_
- Migration and data transfer _(di chuyển lên cloud: database, file, server)_
- Networking, connectivity, and content delivery _(mạng: VPC, DNS, CDN, hybrid)_
- Resiliency _(khả năng chịu lỗi: Multi-AZ, failover, loose coupling)_
- Security _(bảo mật: IAM, encryption, threat detection, compliance)_
- Serverless and event-driven design principles _(không server, hướng sự kiện: Lambda, SQS, EventBridge)_
- Storage _(lưu trữ: object, block, file, archive)_

## In-Scope Services Theo Nhóm

Đây là danh sách trọng tâm được rút gọn từ Exam Guide. Không phải service nào cũng xuất hiện nhiều như nhau; hãy ưu tiên core services trước.

### Rất Quan Trọng

- [IAM](services-overview.md#iam), [IAM Identity Center](services-overview.md#iam-identity-center), [Organizations](services-overview.md#organizations), [Control Tower](services-overview.md#control-tower), [SCP](services-overview.md#scp)
- [VPC](services-overview.md#vpc), [subnet](services-overview.md#subnet), [route table](services-overview.md#route-table), [security group](services-overview.md#security-group), [network ACL](services-overview.md#network-acl), [NAT Gateway](services-overview.md#nat-gateway), [VPC endpoints](services-overview.md#vpc-endpoints)
- ELB: [ALB](services-overview.md#elb--alb--nlb--gateway-load-balancer), [NLB](services-overview.md#elb--alb--nlb--gateway-load-balancer), [Gateway Load Balancer](services-overview.md#elb--alb--nlb--gateway-load-balancer)
- [Route 53](services-overview.md#route-53), [CloudFront](services-overview.md#cloudfront), [Global Accelerator](services-overview.md#global-accelerator), [Direct Connect](services-overview.md#direct-connect), [Site-to-Site VPN](services-overview.md#site-to-site-vpn), [Transit Gateway](services-overview.md#transit-gateway), [PrivateLink](services-overview.md#privatelink)
- [EC2](services-overview.md#ec2), [Auto Scaling](services-overview.md#auto-scaling), [Launch Template](services-overview.md#launch-template), [placement group](services-overview.md#placement-group), [Spot](services-overview.md#spot-instances), [Reserved Instances](services-overview.md#reserved-instances), [Savings Plans](services-overview.md#savings-plans)
- [Lambda](services-overview.md#lambda), [Fargate](services-overview.md#fargate), [ECS](services-overview.md#ecs), [EKS](services-overview.md#eks), [ECR](services-overview.md#ecr)
- [S3](services-overview.md#s3), [S3 Glacier](services-overview.md#s3-glacier), [EBS](services-overview.md#ebs), [EFS](services-overview.md#efs), [FSx](services-overview.md#fsx), [AWS Backup](services-overview.md#aws-backup), [Storage Gateway](services-overview.md#storage-gateway)
- [RDS](services-overview.md#rds), [Aurora](services-overview.md#aurora), [DynamoDB](services-overview.md#dynamodb), [ElastiCache](services-overview.md#elasticache), [Redshift](services-overview.md#redshift)
- [SQS](services-overview.md#sqs), [SNS](services-overview.md#sns), [EventBridge](services-overview.md#eventbridge), [Step Functions](services-overview.md#step-functions), [API Gateway](services-overview.md#api-gateway), [AppSync](services-overview.md#appsync)
- [CloudWatch](services-overview.md#cloudwatch), [CloudTrail](services-overview.md#cloudtrail), [Config](services-overview.md#config), [Systems Manager](services-overview.md#systems-manager), [Trusted Advisor](services-overview.md#trusted-advisor), [Compute Optimizer](services-overview.md#compute-optimizer)
- [KMS](services-overview.md#kms), [ACM](services-overview.md#acm), [Secrets Manager](services-overview.md#secrets-manager), [WAF](services-overview.md#waf), [Shield](services-overview.md#shield), [Cognito](services-overview.md#cognito), [GuardDuty](services-overview.md#guardduty), [Macie](services-overview.md#macie), [Inspector](services-overview.md#inspector), [Security Hub](services-overview.md#security-hub)
- [DataSync](services-overview.md#datasync), [DMS](services-overview.md#dms), [Snow Family](services-overview.md#snow-family), [Transfer Family](services-overview.md#transfer-family)

### Nên Biết Để Không Mất Điểm

- [Athena](services-overview.md#athena), [Glue](services-overview.md#glue), [Kinesis](services-overview.md#kinesis), [EMR](services-overview.md#emr), [Lake Formation](services-overview.md#lake-formation), [QuickSight](services-overview.md#quicksight), [OpenSearch](services-overview.md#opensearch), [MSK](services-overview.md#msk)
- [DocumentDB](services-overview.md#documentdb), [Neptune](services-overview.md#neptune), [Keyspaces](services-overview.md#keyspaces), [QLDB](services-overview.md#qldb)
- [AppFlow](services-overview.md#appflow), [Amazon MQ](services-overview.md#amazon-mq)
- [Elastic Beanstalk](services-overview.md#elastic-beanstalk), [Batch](services-overview.md#batch), [Outposts](services-overview.md#outposts), [Wavelength](services-overview.md#wavelength), [VMware Cloud on AWS](services-overview.md#vmware-cloud-on-aws)
- [CloudFormation](services-overview.md#cloudformation), [Service Catalog](services-overview.md#service-catalog), [Well-Architected Tool](services-overview.md#well-architected-tool)
- [Comprehend](services-overview.md#aiml-services-nên-biết--mức-use-case), [Polly](services-overview.md#aiml-services-nên-biết--mức-use-case), [Rekognition](services-overview.md#aiml-services-nên-biết--mức-use-case), [Textract](services-overview.md#aiml-services-nên-biết--mức-use-case), [Transcribe](services-overview.md#aiml-services-nên-biết--mức-use-case), [Translate](services-overview.md#aiml-services-nên-biết--mức-use-case), [SageMaker](services-overview.md#aiml-services-nên-biết--mức-use-case) ở mức use case

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

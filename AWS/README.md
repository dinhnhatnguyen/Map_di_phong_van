# AWS SAA-C03 Study Kit

Bộ tài liệu này được tổ chức để học AWS Certified Solutions Architect - Associate SAA-C03 theo hướng dễ nhớ, bám blueprint chính thức, và luyện bằng scenario giống cách đề thi hỏi.

> Cập nhật theo tài liệu chính thức AWS SAA-C03 Exam Guide Version 1.1 và trang AWS Certification truy cập ngày 2026-04-26. Đây là tài liệu học, không phải exam dump.

## Cách Học Nhanh

1. Đọc [SAA-C03 Blueprint](00-start-here/SAA-C03-blueprint.md) để biết domain, trọng số, format đề.
2. Đi theo [Learning Roadmap](00-start-here/learning-roadmap.md) trong 21 hoặc 30 ngày.
3. Học từng nhóm service ở [Core Services](01-core-services/).
4. Dùng [Service Selection Matrix](02-architecture/service-selection-matrix.md) để luyện chọn service theo requirement.
5. Làm mock ở [Exam Practice](03-exam-practice/) và ghi lỗi vào [Wrong Answer Log](04-checklists/wrong-answer-log.md).
6. Trước ngày thi, ôn [Final Review Checklist](04-checklists/final-review-checklist.md) và [Flashcards](04-checklists/flashcards.md).

## Bản Đồ Tài Liệu

| Mục tiêu | File nên đọc |
|---|---|
| Hiểu đề thi, domain, cách chấm | [00-start-here/SAA-C03-blueprint.md](00-start-here/SAA-C03-blueprint.md) |
| Lên lịch học | [00-start-here/learning-roadmap.md](00-start-here/learning-roadmap.md) |
| Compute, container, serverless | [01-core-services/compute.md](01-core-services/compute.md) |
| S3, EBS, EFS, FSx, backup | [01-core-services/storage.md](01-core-services/storage.md) |
| VPC, subnet, ALB/NLB, Route 53, CloudFront, hybrid network | [01-core-services/networking.md](01-core-services/networking.md) |
| RDS, Aurora, DynamoDB, ElastiCache, Redshift, purpose-built DB | [01-core-services/databases.md](01-core-services/databases.md) |
| IAM, KMS, Cognito, WAF, Shield, GuardDuty, Secrets Manager | [01-core-services/security-identity.md](01-core-services/security-identity.md) |
| SQS, SNS, EventBridge, Step Functions, CloudWatch, CloudTrail, cost | [01-core-services/integration-observability-cost.md](01-core-services/integration-observability-cost.md) |
| Migration, analytics, edge, ML services thường gặp | [01-core-services/migration-analytics-edge.md](01-core-services/migration-analytics-edge.md) |
| Kiến trúc mẫu và diagram | [02-architecture/architecture-patterns.md](02-architecture/architecture-patterns.md) |
| Chọn service theo keyword đề | [02-architecture/service-selection-matrix.md](02-architecture/service-selection-matrix.md) |
| So sánh dễ nhầm | [02-architecture/high-yield-comparisons.md](02-architecture/high-yield-comparisons.md) |
| Luyện đề scenario | [03-exam-practice/mock-exam-01.md](03-exam-practice/mock-exam-01.md), [03-exam-practice/mock-exam-02.md](03-exam-practice/mock-exam-02.md) |
| Nhận diện bẫy đề | [03-exam-practice/question-patterns-and-keywords.md](03-exam-practice/question-patterns-and-keywords.md) |

## Cấu Trúc Theo Blueprint

| Domain SAA-C03 | Trọng số | Nên học ở đâu |
|---|---:|---|
| Design Secure Architectures | 30% | Security, Networking, Storage, Databases |
| Design Resilient Architectures | 26% | Architecture Patterns, Compute, Databases, Integration |
| Design High-Performing Architectures | 24% | Compute, Storage, Databases, Networking, Analytics |
| Design Cost-Optimized Architectures | 20% | Cost, Storage, Compute, Databases, Networking |

## Nguyên Tắc Làm Đề

- Đọc requirement cuối câu trước: secure, cheapest, least operational overhead, lowest latency, highest availability.
- Tìm constraint: cannot modify app, on-premises, multi-Region, millions of requests, unpredictable traffic, compliance retention.
- Chọn managed service khi đề nhấn mạnh giảm vận hành.
- Multi-AZ thường là high availability trong một Region; multi-Region là disaster recovery/global latency.
- Read Replica là scale đọc; Multi-AZ là failover. Đề hay gài chỗ này.
- SQS là decouple bằng queue; SNS là fan-out/pub-sub; EventBridge là event routing theo rule; Step Functions là workflow nhiều bước.
- S3 là object storage; EBS là block cho một EC2/AZ; EFS là shared NFS đa AZ; FSx là managed file system chuyên biệt.

## Nguồn Chính

- [AWS Certified Solutions Architect - Associate certification page](https://aws.amazon.com/certification/certified-solutions-architect-associate/)
- [AWS SAA-C03 Exam Guide PDF](https://d1.awsstatic.com/training-and-certification/docs-sa-assoc/AWS-Certified-Solutions-Architect-Associate_Exam-Guide.pdf)
- [AWS SAA-C03 user guide page](https://docs.aws.amazon.com/aws-certification/latest/userguide/solutions-architect-associate-03.html)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

## Bản Nháp Gốc

Hai tài liệu ban đầu được giữ lại tại:

- [archive/original-drafts/aws_SAA_C03_handbook.md](archive/original-drafts/aws_SAA_C03_handbook.md)
- [archive/original-drafts/aws_architecture_combos.md](archive/original-drafts/aws_architecture_combos.md)

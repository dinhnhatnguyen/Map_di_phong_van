# Architecture Patterns

> Gặp từ viết tắt không quen? Xem **[Từ Điển](../00-start-here/glossary.md)**.

File này gom các kiến trúc hay gặp trong SAA-C03. Khi luyện đề, hãy tập đọc requirement rồi map về một pattern gần nhất.

## 1. Secure 3-Tier Web Architecture

```mermaid
flowchart TD
    classDef internet fill:#dbeafe,stroke:#2563eb,color:#1e3a8a
    classDef edge    fill:#fef3c7,stroke:#d97706,color:#92400e
    classDef lb      fill:#dcfce7,stroke:#16a34a,color:#14532d
    classDef compute fill:#e0f2fe,stroke:#0284c7,color:#0c4a6e
    classDef db      fill:#f3e8ff,stroke:#9333ea,color:#581c87
    classDef storage fill:#cffafe,stroke:#0891b2,color:#164e63
    classDef nat     fill:#f1f5f9,stroke:#64748b,color:#1e293b
    classDef endpoint fill:#e0e7ff,stroke:#4f46e5,color:#312e81

    User((Users)):::internet --> R53[Route 53]:::edge
    R53 --> CF[CloudFront + WAF]:::edge
    CF --> ALB[Public ALB]:::lb

    subgraph VPC[VPC — 2+ AZs]
      subgraph Pub[Public Subnets]
        ALB
        NAT1[NAT Gateway AZ-A]:::nat
        NAT2[NAT Gateway AZ-B]:::nat
      end
      subgraph App[Private App Subnets]
        ASG[EC2 Auto Scaling Group]:::compute
      end
      subgraph Data[Private DB Subnets]
        RDS[(RDS Multi-AZ)]:::db
      end
    end

    ALB --> ASG
    ASG --> RDS
    ASG --> NAT1
    ASG --> NAT2
    ASG --> EP[S3 / DynamoDB VPC Endpoint]:::endpoint
```

Chọn khi:

- Web/app truyền thống.
- Cần HA trong một Region.
- Cần bảo mật: ALB public, app/db private.
- Cần scale EC2 theo tải.

Điểm thi:

- [RDS](../01-core-services/databases.md) Multi-AZ cho failover.
- Read Replica nếu đọc nhiều.
- Security Group DB chỉ allow từ SG app.
- CloudFront + [WAF](../00-start-here/glossary.md#waf) nếu global/web protection.
- [VPC endpoint](../01-core-services/networking.md#vpc-endpoints) để private access S3/DynamoDB và giảm [NAT](../00-start-here/glossary.md#nat) cost.

---

## 2. Static Website Global

```mermaid
flowchart LR
    classDef internet fill:#dbeafe,stroke:#2563eb,color:#1e3a8a
    classDef edge    fill:#fef3c7,stroke:#d97706,color:#92400e
    classDef storage fill:#cffafe,stroke:#0891b2,color:#164e63

    User((Users)):::internet --> R53[Route 53 Alias]:::edge
    R53 --> CF[CloudFront + OAC]:::edge
    CF --> WAF[AWS WAF]:::edge
    CF --> S3[(Private S3 Origin)]:::storage
    S3 --> Logs[(S3 Access Logs)]:::storage
```

Chọn khi:

- React/Vue/Angular/static content.
- Global low latency.
- Low cost, high durability.

Điểm thi:

- [CloudFront Origin Access Control (OAC)](../00-start-here/glossary.md#oac--oai) giữ S3 không public.
- [ACM](../00-start-here/glossary.md#acm) certificate cho CloudFront phải tạo ở **us-east-1**.
- S3 lifecycle cho logs/assets cũ.

---

## 3. Serverless API

```mermaid
flowchart LR
    classDef internet  fill:#dbeafe,stroke:#2563eb,color:#1e3a8a
    classDef edge      fill:#fef3c7,stroke:#d97706,color:#92400e
    classDef compute   fill:#e0f2fe,stroke:#0284c7,color:#0c4a6e
    classDef db        fill:#f3e8ff,stroke:#9333ea,color:#581c87
    classDef security  fill:#fce7f3,stroke:#db2777,color:#831843
    classDef integration fill:#fff7ed,stroke:#ea580c,color:#7c2d12

    Client((Client)):::internet --> APIGW[API Gateway]:::edge
    APIGW --> Lambda[AWS Lambda]:::compute
    Lambda --> DDB[(DynamoDB)]:::db
    Lambda --> SM[Secrets Manager]:::security
    DDB --> Streams[DynamoDB Streams]:::integration
    Streams --> Worker[Lambda Worker]:::compute
```

Chọn khi:

- Traffic biến động.
- Muốn least operational overhead.
- Event-driven API ngắn.

Điểm thi:

- Lambda timeout tối đa 15 phút ([Cold start](../00-start-here/glossary.md#cold-start) giảm bằng Provisioned Concurrency).
- DynamoDB on-demand cho unpredictable traffic.
- [DAX](../00-start-here/glossary.md#dax) cho read-heavy low latency.
- RDS Proxy nếu Lambda gọi [RDS](../00-start-here/glossary.md#rds).

---

## 4. Fan-Out Event Processing

```mermaid
flowchart LR
    classDef integration fill:#fff7ed,stroke:#ea580c,color:#7c2d12
    classDef compute     fill:#e0f2fe,stroke:#0284c7,color:#0c4a6e
    classDef storage     fill:#cffafe,stroke:#0891b2,color:#164e63

    Event([Order Created]):::integration --> SNS[SNS Topic]:::integration
    SNS --> Q1[SQS Email Queue]:::integration
    SNS --> Q2[SQS Inventory Queue]:::integration
    SNS --> Q3[SQS Analytics Queue]:::integration
    Q1 --> L1[Lambda — Email]:::compute
    Q2 --> W2[ECS Worker — Inventory]:::compute
    Q3 --> KF[Kinesis Firehose]:::integration
    KF --> S3[(S3 Data Lake)]:::storage
```

Chọn khi:

- Một event cần nhiều consumer độc lập.
- Không muốn consumer ảnh hưởng nhau.
- Cần DLQ/retry theo từng workflow.

Điểm thi:

- [SNS](../00-start-here/glossary.md#sns) [fan-out](../00-start-here/glossary.md#fan-out).
- [SQS](../00-start-here/glossary.md#sqs) per consumer để [decouple](../00-start-here/glossary.md#decouple--loose-coupling).
- [DLQ](../00-start-here/glossary.md#dlq) để xử lý lỗi.

---

## 5. Queue-Based Worker Scaling

```mermaid
flowchart LR
    classDef compute     fill:#e0f2fe,stroke:#0284c7,color:#0c4a6e
    classDef integration fill:#fff7ed,stroke:#ea580c,color:#7c2d12
    classDef db          fill:#f3e8ff,stroke:#9333ea,color:#581c87
    classDef edge        fill:#fef3c7,stroke:#d97706,color:#92400e

    Prod[Producers]:::compute --> SQS[SQS Queue]:::integration
    SQS --> ASG[Auto Scaling Workers]:::compute
    ASG --> DB[(RDS / DynamoDB)]:::db
    CW[CloudWatch\nApproximateNumberOfMessages]:::edge --> ASG
    SQS --> DLQ[Dead Letter Queue]:::integration
```

Chọn khi:

- Traffic spike.
- Worker xử lý lâu.
- Cần backpressure.

Điểm thi:

- Scale worker theo queue depth (metric `ApproximateNumberOfMessages`).
- [Visibility Timeout](../00-start-here/glossary.md#visibility-timeout) > max processing time.
- [DLQ](../00-start-here/glossary.md#dlq) after maxReceiveCount.

---

## 6. Container Microservices

```mermaid
flowchart TD
    classDef internet fill:#dbeafe,stroke:#2563eb,color:#1e3a8a
    classDef lb       fill:#dcfce7,stroke:#16a34a,color:#14532d
    classDef compute  fill:#e0f2fe,stroke:#0284c7,color:#0c4a6e
    classDef db       fill:#f3e8ff,stroke:#9333ea,color:#581c87
    classDef storage  fill:#cffafe,stroke:#0891b2,color:#164e63
    classDef integration fill:#fff7ed,stroke:#ea580c,color:#7c2d12

    User((Users)):::internet --> ALB[ALB]:::lb
    ALB --> ECS[ECS Service — Fargate]:::compute
    ECR[(ECR Image Registry)]:::storage --> ECS
    ECS --> DDB[(DynamoDB)]:::db
    ECS --> Aurora[(Aurora)]:::db
    ECS --> SQS[SQS]:::integration
```

Chọn khi:

- App đã containerized.
- Cần giảm quản lý EC2.
- Microservices scale độc lập.

Điểm thi:

- Fargate = less ops.
- ECS on EC2 = more control/cost tuning.
- EKS nếu yêu cầu Kubernetes.

---

## 7. Hybrid Storage

```mermaid
flowchart LR
    classDef onprem   fill:#fefce8,stroke:#ca8a04,color:#713f12
    classDef storage  fill:#cffafe,stroke:#0891b2,color:#164e63
    classDef migration fill:#ecfdf5,stroke:#059669,color:#064e3b

    OnPrem[On-Premises\nFile Servers]:::onprem --> SG[Storage Gateway\nFile Gateway]:::migration
    SG --> S3[(S3)]:::storage
    OnPrem --> DS[DataSync Agent]:::migration
    DS --> EFS[EFS]:::storage
    DS --> FSx[FSx]:::storage
    DS --> S3
```

Chọn khi:

- On-prem app cần NFS/SMB local interface: Storage Gateway.
- Cần migrate/sync file nhanh: DataSync.
- Cần archive tape replacement: Tape Gateway.

---

## 8. Database Migration With Low Downtime

```mermaid
flowchart LR
    classDef onprem   fill:#fefce8,stroke:#ca8a04,color:#713f12
    classDef migration fill:#ecfdf5,stroke:#059669,color:#064e3b
    classDef db        fill:#f3e8ff,stroke:#9333ea,color:#581c87
    classDef compute   fill:#e0f2fe,stroke:#0284c7,color:#0c4a6e

    Src[(On-Prem DB)]:::onprem --> SCT[AWS Schema\nConversion Tool]:::migration
    Src --> DMS[AWS DMS — CDC]:::migration
    SCT --> Target[(Aurora / RDS)]:::db
    DMS --> Target
    App[Application]:::compute -. cutover .-> Target
```

Chọn khi:

- Migrating database to AWS.
- Minimal downtime.
- Homogeneous hoặc heterogeneous migration.

Điểm thi:

- Heterogeneous: [SCT](../00-start-here/glossary.md#sct) + [DMS](../00-start-here/glossary.md#dms).
- Ongoing replication/[CDC](../00-start-here/glossary.md#cdc) để giảm downtime.

---

## 9. Multi-Region Disaster Recovery

```mermaid
flowchart TB
    classDef internet    fill:#dbeafe,stroke:#2563eb,color:#1e3a8a
    classDef edge        fill:#fef3c7,stroke:#d97706,color:#92400e
    classDef compute     fill:#e0f2fe,stroke:#0284c7,color:#0c4a6e
    classDef db          fill:#f3e8ff,stroke:#9333ea,color:#581c87
    classDef storage     fill:#cffafe,stroke:#0891b2,color:#164e63

    Users((Users)):::internet --> R53[Route 53 Failover /\nLatency Routing]:::edge

    subgraph Primary[Primary Region]
      PApp[App Tier]:::compute
      PDB[(Aurora Global /\nDynamoDB Global — Writer)]:::db
      PS3[(S3 — CRR Source)]:::storage
    end

    subgraph Secondary[Secondary Region]
      SApp[App Tier\n— standby]:::compute
      SDB[(Aurora Global /\nDynamoDB Global — Reader)]:::db
      SS3[(S3 — CRR Target)]:::storage
    end

    R53 --> Primary
    R53 -.failover.-> Secondary
    PDB <-->|global replication| SDB
    PS3 -->|Cross-Region Replication| SS3
```

DR strategies:

| Strategy | Cost | [RTO](../00-start-here/glossary.md#rto) / [RPO](../00-start-here/glossary.md#rpo) | Khi dùng |
|---|---:|---:|---|
| Backup and restore | Thấp | Cao | Cost-first, downtime chấp nhận được |
| [Pilot light](../00-start-here/glossary.md#pilot-light) | Thấp-vừa | Trung bình | Core data/services always ready |
| [Warm standby](../00-start-here/glossary.md#warm-standby) | Vừa-cao | Thấp | Reduced-capacity environment ready |
| [Active-active](../00-start-here/glossary.md#active-active) | Cao | Rất thấp | Global critical workload |

Điểm thi:

- [RTO](../00-start-here/glossary.md#rto) = thời gian khôi phục dịch vụ.
- [RPO](../00-start-here/glossary.md#rpo) = lượng dữ liệu có thể mất.
- [Multi-AZ](../00-start-here/glossary.md#multi-az) không thay thế [Multi-Region](../00-start-here/glossary.md#multi-region) DR.

---

## 10. Data Lake Analytics

```mermaid
flowchart LR
    classDef integration fill:#fff7ed,stroke:#ea580c,color:#7c2d12
    classDef migration   fill:#ecfdf5,stroke:#059669,color:#064e3b
    classDef storage     fill:#cffafe,stroke:#0891b2,color:#164e63
    classDef analytics   fill:#fffbeb,stroke:#d97706,color:#78350f

    Src1[Apps / Logs]:::integration --> KF[Kinesis Firehose]:::integration
    Src2[On-Prem Files]:::migration --> DS[DataSync]:::migration
    KF --> S3[(S3 Data Lake)]:::storage
    DS --> S3
    S3 --> Glue[Glue Crawler\n+ Catalog + ETL]:::analytics
    Glue --> Athena[Athena\nServerless SQL]:::analytics
    Athena --> QS[QuickSight\nDashboard]:::analytics
```

Chọn khi:

- Logs/events/file data tập trung ở S3.
- Query serverless bằng Athena.
- Transform/catalog bằng Glue.
- Dashboard bằng QuickSight.

---

## 11. Private AWS Service Access

```mermaid
flowchart TD
    classDef compute  fill:#e0f2fe,stroke:#0284c7,color:#0c4a6e
    classDef endpoint fill:#e0e7ff,stroke:#4f46e5,color:#312e81
    classDef storage  fill:#cffafe,stroke:#0891b2,color:#164e63
    classDef security fill:#fce7f3,stroke:#db2777,color:#831843
    classDef edge     fill:#fef3c7,stroke:#d97706,color:#92400e

    subgraph PrivSub[Private Subnet]
      App[EC2 / Lambda / ECS]:::compute
    end

    App --> GW[Gateway Endpoint\nS3 · DynamoDB]:::endpoint
    App --> IF[Interface Endpoint\nSecrets Manager · KMS\nCloudWatch · SSM]:::endpoint
    GW --> S3[(S3)]:::storage
    IF --> AWSSVC[AWS Service\nvia PrivateLink]:::edge
```

Chọn khi:

- Private subnet cần gọi AWS services không qua internet.
- Muốn giảm NAT cost cho S3/DynamoDB.
- Muốn chặt security bằng endpoint policy.

---

## Pattern Selection

| Nếu đề nói                     | Pattern                 |
| ------------------------------ | ----------------------- |
| Web app HA, private database   | Secure 3-tier           |
| Static assets global           | Static website global   |
| No servers, variable traffic   | Serverless API          |
| One event triggers many flows  | Fan-out                 |
| Workers overloaded by spikes   | Queue-based scaling     |
| Docker app, less ops           | Container microservices |
| On-prem files to AWS           | Hybrid storage          |
| DB migration low downtime      | DMS CDC                 |
| Region failure requirement     | Multi-Region DR         |
| Query logs in S3               | Data lake analytics     |
| Private subnet calling AWS API | VPC endpoints           |

## Liên Kết

- [Service Selection Matrix](service-selection-matrix.md)
- [High-Yield Comparisons](high-yield-comparisons.md)
- [Mock Exam 01](../03-exam-practice/mock-exam-01.md)

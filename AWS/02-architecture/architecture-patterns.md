# Architecture Patterns

File này gom các kiến trúc hay gặp trong SAA-C03. Khi luyện đề, hãy tập đọc requirement rồi map về một pattern gần nhất.

## 1. Secure 3-Tier Web Architecture

```mermaid
flowchart TD
    User((Users)) --> R53[Route 53]
    R53 --> CF[CloudFront + WAF]
    CF --> ALB[Public ALB]
    subgraph VPC[VPC across 2+ AZs]
      subgraph Public[Public subnets]
        ALB
        NAT1[NAT Gateway AZ A]
        NAT2[NAT Gateway AZ B]
      end
      subgraph PrivateApp[Private app subnets]
        ASG[EC2 Auto Scaling Group]
      end
      subgraph PrivateData[Private DB subnets]
        RDS[(RDS Multi-AZ)]
      end
    end
    ALB --> ASG
    ASG --> RDS
    ASG --> VPCE[S3/DynamoDB VPC Endpoint]
```

Chọn khi:

- Web/app truyền thống.
- Cần HA trong một Region.
- Cần bảo mật: ALB public, app/db private.
- Cần scale EC2 theo tải.

Điểm thi:

- RDS Multi-AZ cho failover.
- Read Replica nếu đọc nhiều.
- Security Group DB chỉ allow từ SG app.
- CloudFront + WAF nếu global/web protection.
- VPC endpoint để private access S3/DynamoDB và giảm NAT cost.

## 2. Static Website Global

```mermaid
flowchart LR
    User((Users)) --> R53[Route 53 Alias]
    R53 --> CF[CloudFront]
    CF --> S3[(Private S3 Origin)]
    CF --> WAF[AWS WAF]
    S3 --> Logs[(S3 Access Logs / CloudTrail Data Events)]
```

Chọn khi:

- React/Vue/Angular/static content.
- Global low latency.
- Low cost, high durability.

Điểm thi:

- Use CloudFront Origin Access Control để S3 không public.
- ACM certificate cho CloudFront ở us-east-1.
- S3 lifecycle cho logs/assets cũ.

## 3. Serverless API

```mermaid
flowchart LR
    Client((Client)) --> APIGW[API Gateway]
    APIGW --> Lambda[AWS Lambda]
    Lambda --> DDB[(DynamoDB)]
    Lambda --> SM[Secrets Manager]
    DDB --> Streams[DynamoDB Streams]
    Streams --> Worker[Lambda Worker]
```

Chọn khi:

- Traffic biến động.
- Muốn least operational overhead.
- Event-driven API ngắn.

Điểm thi:

- Lambda timeout tối đa 15 phút.
- DynamoDB on-demand cho unpredictable traffic.
- DAX cho read-heavy low latency.
- RDS Proxy nếu Lambda gọi RDS.

## 4. Fan-Out Event Processing

```mermaid
flowchart LR
    Event[Order Created] --> SNS[SNS Topic]
    SNS --> Q1[SQS Email Queue]
    SNS --> Q2[SQS Inventory Queue]
    SNS --> Q3[SQS Analytics Queue]
    Q1 --> L1[Lambda Email]
    Q2 --> W2[EC2/ECS Worker]
    Q3 --> Firehose[Kinesis Firehose]
    Firehose --> S3[(S3 Data Lake)]
```

Chọn khi:

- Một event cần nhiều consumer độc lập.
- Không muốn consumer ảnh hưởng nhau.
- Cần DLQ/retry theo từng workflow.

Điểm thi:

- SNS fan-out.
- SQS per consumer để decouple.
- DLQ để xử lý lỗi.

## 5. Queue-Based Worker Scaling

```mermaid
flowchart LR
    Producer[Producers] --> SQS[SQS Queue]
    SQS --> ASG[Auto Scaling Workers]
    ASG --> DB[(RDS/DynamoDB)]
    CW[CloudWatch ApproximateNumberOfMessages] --> ASG
    SQS --> DLQ[Dead Letter Queue]
```

Chọn khi:

- Traffic spike.
- Worker xử lý lâu.
- Cần backpressure.

Điểm thi:

- Scale worker theo queue depth.
- Visibility timeout > max processing time.
- DLQ after maxReceiveCount.

## 6. Container Microservices

```mermaid
flowchart TD
    User((Users)) --> ALB[ALB]
    ALB --> ECS[ECS Service on Fargate]
    ECS --> DDB[(DynamoDB)]
    ECS --> Aurora[(Aurora)]
    ECS --> SQS[SQS]
    ECR[ECR] --> ECS
```

Chọn khi:

- App đã containerized.
- Cần giảm quản lý EC2.
- Microservices scale độc lập.

Điểm thi:

- Fargate = less ops.
- ECS on EC2 = more control/cost tuning.
- EKS nếu yêu cầu Kubernetes.

## 7. Hybrid Storage

```mermaid
flowchart LR
    OnPrem[On-prem file servers] --> SG[Storage Gateway File Gateway]
    SG --> S3[(S3)]
    OnPrem --> DS[DataSync Agent]
    DS --> EFS[EFS/FSx/S3]
```

Chọn khi:

- On-prem app cần NFS/SMB local interface: Storage Gateway.
- Cần migrate/sync file nhanh: DataSync.
- Cần archive tape replacement: Tape Gateway.

## 8. Database Migration With Low Downtime

```mermaid
flowchart LR
    Source[(On-prem DB)] --> DMS[AWS DMS CDC]
    SCT[AWS Schema Conversion Tool] --> Target[(Aurora/RDS)]
    DMS --> Target
    App[Application] -. cutover .-> Target
```

Chọn khi:

- Migrating database to AWS.
- Minimal downtime.
- Homogeneous hoặc heterogeneous migration.

Điểm thi:

- Heterogeneous: SCT + DMS.
- Ongoing replication/CDC để giảm downtime.

## 9. Multi-Region Disaster Recovery

```mermaid
flowchart TB
    Users((Users)) --> R53[Route 53 Failover/Latency]
    R53 --> Primary[Primary Region]
    R53 --> Secondary[Secondary Region]
    Primary --> S3A[(S3 CRR)]
    S3A --> S3B[(S3 Secondary)]
    Primary --> DB1[(Aurora Global / DynamoDB Global)]
    DB1 <--> DB2[(Secondary DB)]
```

DR strategies:

| Strategy | Cost | RTO/RPO | Khi dùng |
|---|---:|---:|---|
| Backup and restore | Thấp | Cao | Cost-first, downtime chấp nhận được |
| Pilot light | Thấp-vừa | Trung bình | Core data/services always ready |
| Warm standby | Vừa-cao | Thấp | Reduced-capacity environment ready |
| Active-active | Cao | Rất thấp | Global critical workload |

Điểm thi:

- RTO = thời gian khôi phục.
- RPO = lượng dữ liệu có thể mất.
- Multi-AZ không thay thế multi-Region DR.

## 10. Data Lake Analytics

```mermaid
flowchart LR
    Sources[Apps/Logs/Streams] --> Firehose[Kinesis Firehose]
    Sources --> DataSync[DataSync]
    Firehose --> S3[(S3 Data Lake)]
    DataSync --> S3
    Glue[Glue Crawler/Catalog/ETL] --> S3
    Athena[Athena] --> S3
    QuickSight[QuickSight] --> Athena
```

Chọn khi:

- Logs/events/file data tập trung ở S3.
- Query serverless bằng Athena.
- Transform/catalog bằng Glue.
- Dashboard bằng QuickSight.

## 11. Private AWS Service Access

```mermaid
flowchart TD
    subgraph PrivateSubnet[Private Subnet]
      App[EC2/Lambda/ECS]
    end
    App --> GWEP[Gateway Endpoint: S3/DynamoDB]
    App --> IFEP[Interface Endpoint: Secrets Manager/KMS/CloudWatch]
    GWEP --> S3[(S3)]
    IFEP --> AWSService[AWS Service PrivateLink]
```

Chọn khi:

- Private subnet cần gọi AWS services không qua internet.
- Muốn giảm NAT cost cho S3/DynamoDB.
- Muốn chặt security bằng endpoint policy.

## Pattern Selection

| Nếu đề nói | Pattern |
|---|---|
| Web app HA, private database | Secure 3-tier |
| Static assets global | Static website global |
| No servers, variable traffic | Serverless API |
| One event triggers many flows | Fan-out |
| Workers overloaded by spikes | Queue-based scaling |
| Docker app, less ops | Container microservices |
| On-prem files to AWS | Hybrid storage |
| DB migration low downtime | DMS CDC |
| Region failure requirement | Multi-Region DR |
| Query logs in S3 | Data lake analytics |
| Private subnet calling AWS API | VPC endpoints |

## Liên Kết

- [Service Selection Matrix](service-selection-matrix.md)
- [High-Yield Comparisons](high-yield-comparisons.md)
- [Mock Exam 01](../03-exam-practice/mock-exam-01.md)

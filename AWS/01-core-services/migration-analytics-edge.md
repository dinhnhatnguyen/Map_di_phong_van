# Migration, Analytics, Edge, And Specialty Services

> Gặp từ viết tắt không quen? Xem **[Từ Điển](../00-start-here/glossary.md)**.

SAA-C03 không yêu cầu bạn thành chuyên gia analytics/ML, nhưng cần biết service nào giải quyết đúng bài toán migration, ingestion, transformation, visualization, global edge, và vài use case ML managed.

## Migration And Transfer

| Service | Dùng khi | Bẫy đề |
|---|---|---|
| DMS | Migrate database, CDC, homogeneous/heterogeneous | Heterogeneous cần Schema Conversion Tool |
| Application Migration Service | Lift-and-shift server sang AWS | Dùng cho app/server migration, không phải DB-specific |
| DataSync | Online transfer file/object giữa on-prem/NFS/SMB/S3/EFS/FSx | Managed, nhanh hơn tự rsync |
| Storage Gateway | Hybrid storage appliance: file/volume/tape | On-prem app cần interface local |
| Snow Family | Offline/edge transfer TB-PB, limited bandwidth | Thiết bị vật lý gửi đến AWS |
| Transfer Family | Managed SFTP/FTPS/FTP vào S3/EFS | Khi partner/client cần SFTP |
| Migration Hub | Theo dõi migration | Không tự migrate mọi thứ |
| Application Discovery Service | Discover on-prem inventory/dependency | Giai đoạn assess |

Nguồn: [AWS Migration and Transfer](https://docs.aws.amazon.com/migrationhub/), [AWS DMS](https://docs.aws.amazon.com/dms/), [AWS DataSync](https://docs.aws.amazon.com/datasync/), [AWS Snow Family](https://docs.aws.amazon.com/snowball/).

### Decision Rules

| Requirement | Chọn |
|---|---|
| Database migration with minimal downtime | DMS with ongoing replication/CDC |
| SQL Server to Aurora PostgreSQL | SCT + DMS |
| Move many files online from on-prem NFS to S3 | DataSync |
| On-prem apps keep writing NFS/SMB while data lands in S3 | Storage Gateway File Gateway |
| Backup tape replacement | Storage Gateway Tape Gateway |
| 100 TB over slow internet | Snowball Edge |
| Partner uploads using SFTP | Transfer Family |
| Lift-and-shift VM/server | Application Migration Service |

## Analytics And Data

| Service | Dùng khi |
|---|---|
| Athena | Serverless SQL query data in S3 |
| Glue | ETL, Data Catalog, transform CSV to Parquet |
| Lake Formation | Governed data lake permissions |
| Kinesis Data Streams | Realtime streaming ingestion, custom consumers |
| Kinesis Data Firehose | Load streaming data to S3/Redshift/OpenSearch without managing consumers |
| Kinesis Video Streams | Video stream ingestion |
| MSK | Managed Apache Kafka |
| EMR | Managed Hadoop/Spark/Hive big data cluster |
| Redshift | Data warehouse OLAP |
| QuickSight | BI dashboard/visualization |
| OpenSearch | Search/log analytics |
| Data Exchange | Subscribe/share third-party datasets |
| Data Pipeline | Older orchestration/data movement service |

### Athena vs Redshift vs EMR

| Requirement | Chọn |
|---|---|
| Query S3 ad-hoc, pay per query | Athena |
| Enterprise data warehouse, repeated analytics, BI | Redshift |
| Spark/Hadoop big data processing control | EMR |
| ETL and catalog metadata | Glue |
| Dashboard for business users | QuickSight |

### Kinesis vs SQS

| Requirement | Chọn |
|---|---|
| Ordered stream replay by shard/time | Kinesis Data Streams |
| Durable queue for workers | SQS |
| Load stream directly to S3/Redshift/OpenSearch | Firehose |
| Kafka-compatible workloads | MSK |

## Edge And Content Delivery

| Service | Dùng khi |
|---|---|
| CloudFront | CDN/cache HTTP content globally |
| Global Accelerator | Static anycast IP, TCP/UDP acceleration, fast regional failover |
| Route 53 | DNS routing/health checks |
| Wavelength | 5G/telco ultra-low latency |
| Outposts | AWS services on-premises |

## Front-End And Mobile

| Service | Dùng khi |
|---|---|
| Amplify | Build/deploy web/mobile app quickly |
| API Gateway | API front door |
| AppSync | GraphQL/realtime |
| Cognito | User authentication/authorization |
| Pinpoint | User engagement messaging |
| Device Farm | Test mobile/web app on devices |

## Machine Learning Services At SAA Level

Bạn không cần biết training model sâu. Hãy nhớ use case:

| Service | Use case |
|---|---|
| Comprehend | NLP, sentiment/entity/key phrase |
| Forecast | Time-series forecasting |
| Fraud Detector | Fraud detection |
| Kendra | Enterprise search |
| Lex | Chatbot/voice bot |
| Polly | Text to speech |
| Rekognition | Image/video analysis |
| SageMaker | Build/train/deploy ML models |
| Textract | Extract text/forms/tables from documents |
| Transcribe | Speech to text |
| Translate | Language translation |

## Exam Decision Rules

| Requirement | Chọn |
|---|---|
| Convert CSV to Parquet in data lake | Glue |
| Query S3 logs with SQL | Athena |
| Stream click events realtime | Kinesis Data Streams |
| Deliver stream to S3 no custom consumer | Kinesis Firehose |
| Kafka managed | MSK |
| Search application logs | OpenSearch |
| BI dashboard | QuickSight |
| Data lake permissions/governance | Lake Formation |
| On-prem file sync to AWS | DataSync |
| Offline huge migration | Snow Family |
| SFTP into S3/EFS | Transfer Family |

## Liên Kết

- [Storage](storage.md)
- [Networking](networking.md)
- [Databases](databases.md)
- [Architecture Patterns](../02-architecture/architecture-patterns.md)

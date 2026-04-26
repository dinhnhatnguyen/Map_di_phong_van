# Service Selection Matrix

Đây là bảng "nhìn requirement chọn service". Dùng sau khi học từng service để luyện phản xạ làm đề.

## Compute

| Requirement | Best fit | Vì sao |
|---|---|---|
| Legacy app cần custom OS | EC2 | Full control |
| Web app stateless scale ngang | ALB + ASG + EC2 | HA + autoscaling |
| Event xử lý ngắn | Lambda | Serverless, event-driven |
| Container without server management | ECS/Fargate | Less operational overhead |
| Kubernetes API/operator | EKS | Managed Kubernetes |
| Batch jobs queue | AWS Batch | Job scheduling + scalable compute |
| Deploy app nhanh | Elastic Beanstalk | Managed app platform |

## Storage

| Requirement | Best fit | Vì sao |
|---|---|---|
| Object/file static/log/backup | S3 | Durable object storage |
| Unknown access pattern | S3 Intelligent-Tiering | Automatic tiering |
| Long-term archive lowest cost | S3 Glacier Deep Archive | Archive tier |
| EC2 disk | EBS | Block storage |
| Shared Linux file system | EFS | Managed NFS |
| Windows SMB share | FSx for Windows | Managed SMB/AD |
| HPC scratch/high throughput | FSx for Lustre | Performance file system |
| Central backup policy | AWS Backup | Managed backup governance |

## Database

| Requirement | Best fit | Vì sao |
|---|---|---|
| SQL managed | RDS | Managed relational DB |
| SQL higher performance/HA | Aurora | Cloud-native relational |
| Read-heavy SQL | Read Replica/Aurora readers | Scale reads |
| DB HA/failover | Multi-AZ | Standby/failover |
| Key-value/document massive scale | DynamoDB | Serverless NoSQL |
| Global active-active NoSQL | DynamoDB Global Tables | Multi-Region writes |
| DynamoDB microsecond read cache | DAX | Purpose-built cache |
| Session/cache/ranking | ElastiCache Redis/Valkey | In-memory |
| Data warehouse | Redshift | OLAP |
| Ad-hoc SQL on S3 | Athena | Serverless query |

## Networking

| Requirement | Best fit | Vì sao |
|---|---|---|
| HTTP routing path/host | ALB | Layer 7 |
| TCP/UDP high throughput/static IP | NLB | Layer 4 |
| Firewall appliances | GWLB | Transparent appliance fleet |
| Private EC2 outbound internet | NAT Gateway | Managed NAT |
| Private S3/DynamoDB access | Gateway endpoint | No internet/NAT |
| Private AWS service API | Interface endpoint | PrivateLink |
| Many VPCs routing | Transit Gateway | Hub-and-spoke |
| Two VPCs simple | VPC Peering | Direct connection |
| Dedicated on-prem link | Direct Connect | Private dedicated connectivity |
| Fast encrypted on-prem link | Site-to-Site VPN | Internet VPN |
| Global cache | CloudFront | CDN |
| Global static anycast IP | Global Accelerator | Network acceleration |
| DNS failover/latency | Route 53 | Routing policies |

## Security

| Requirement | Best fit | Vì sao |
|---|---|---|
| Service access to AWS resources | IAM role | Temporary credentials |
| Human SSO many accounts | IAM Identity Center | Workforce federation |
| Multi-account guardrails | Organizations + SCP | Account governance |
| Landing zone | Control Tower | Managed multi-account baseline |
| Encrypt and audit key use | KMS CMK | Key control |
| TLS certificate | ACM | Managed cert |
| App user auth | Cognito | User pools/identity |
| Secret rotation | Secrets Manager | Managed rotation |
| SQL injection/XSS/rate-based block | WAF | Layer 7 firewall |
| DDoS advanced | Shield Advanced | Advanced DDoS |
| S3 sensitive data discovery | Macie | Data classification |
| Threat detection | GuardDuty | Managed detection |
| Vulnerability scanning | Inspector | Workload vulnerabilities |

## Integration

| Requirement | Best fit | Vì sao |
|---|---|---|
| Queue worker decoupling | SQS | Pull queue |
| Fan-out notification | SNS | Pub/sub |
| Event routing by rule | EventBridge | Event bus |
| Multi-step workflow | Step Functions | Orchestration |
| REST/HTTP API | API Gateway | API front door |
| GraphQL/realtime | AppSync | Managed GraphQL |
| Legacy broker protocols | Amazon MQ | RabbitMQ/ActiveMQ |

## Observability And Governance

| Requirement | Best fit |
|---|---|
| Metrics/logs/alarms | CloudWatch |
| API audit trail | CloudTrail |
| Resource compliance/history | AWS Config |
| Distributed trace | X-Ray |
| Patch/run command/session | Systems Manager |
| IaC | CloudFormation |
| Best practice recommendations | Trusted Advisor |
| Rightsizing recommendations | Compute Optimizer |
| Review workload | Well-Architected Tool |

## Cost Optimization

| Requirement | Best fit |
|---|---|
| Analyze spend | Cost Explorer |
| Alert on spend | Budgets |
| Detailed billing export | Cost and Usage Report |
| Tag team/project cost | Cost allocation tags |
| Stable compute spend | Savings Plans |
| Stable DB/EC2 instance | Reserved Instances |
| Interruptible job | Spot |
| Storage age-based tiering | S3 Lifecycle |
| Unknown storage access | S3 Intelligent-Tiering |
| Reduce NAT data cost to S3/DDB | Gateway VPC endpoint |

## Migration And Analytics

| Requirement | Best fit |
|---|---|
| DB migration low downtime | DMS |
| DB engine conversion | SCT + DMS |
| File sync on-prem to AWS | DataSync |
| On-prem file/tape/volume cloud-backed | Storage Gateway |
| Huge offline data transfer | Snow Family |
| SFTP/FTPS/FTP into AWS | Transfer Family |
| Lift-and-shift servers | Application Migration Service |
| Stream ingestion | Kinesis Data Streams |
| Stream to S3/Redshift/OpenSearch | Kinesis Firehose |
| ETL/catalog | Glue |
| BI dashboard | QuickSight |

## Shortcut

Khi đề có cụm "least operational overhead", ưu tiên:

- Lambda thay EC2 nếu workload ngắn/event-driven.
- Fargate thay ECS on EC2 nếu không cần host control.
- DynamoDB/Aurora Serverless thay DB tự quản nếu phù hợp data model.
- S3 lifecycle/Intelligent-Tiering thay manual movement.
- AWS Backup thay script backup.
- Managed migration service như DMS/DataSync thay tự viết sync.

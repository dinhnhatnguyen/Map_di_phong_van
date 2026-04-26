# Integration, Observability, Governance, Cost

> Gặp từ viết tắt không quen? Xem **[Từ Điển](../00-start-here/glossary.md)**.

Nhóm này giúp kiến trúc bền, dễ vận hành và tối ưu chi phí. Đề SAA-C03 rất thích cụm từ "loosely coupled", "least operational overhead", "monitor", "audit", "cost-effective".

## Application Integration Map

| Service | Dùng khi | Bẫy đề |
|---|---|---|
| SQS Standard | Queue decoupling, high throughput, at-least-once | Có thể duplicate/out-of-order |
| SQS FIFO | Cần ordering và exactly-once processing semantics | Throughput thấp hơn standard, cần message group |
| SNS | Pub/sub fan-out push tới nhiều subscriber | Không lưu message lâu như queue |
| EventBridge | Event bus, rule-based routing SaaS/AWS/custom events | Tốt cho event routing, không thay queue worker |
| Step Functions | Workflow nhiều bước, retry, branching, human/debug visibility | Tránh Lambda gọi Lambda lồng nhau |
| API Gateway | Managed API front door cho Lambda/HTTP/AWS service | REST/HTTP/WebSocket APIs khác tính năng/cost |
| AppSync | Managed GraphQL API, realtime/subscription | Không chọn nếu đề chỉ cần REST đơn giản |
| Amazon MQ | Managed RabbitMQ/ActiveMQ | Dùng khi app legacy cần protocol broker cụ thể |
| AppFlow | SaaS data integration | Salesforce/SaaS sync tới AWS |

Nguồn: [Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/), [Amazon SNS](https://docs.aws.amazon.com/sns/), [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/), [AWS Step Functions](https://docs.aws.amazon.com/step-functions/), [Amazon CloudWatch](https://docs.aws.amazon.com/cloudwatch/).

## SQS High-Yield

- Visibility timeout: thời gian message bị ẩn sau khi consumer nhận. Nếu xử lý lâu hơn timeout, message có thể xuất hiện lại. Default 30 giây, max 12 giờ. Visibility timeout phải lớn hơn processing time để tránh duplicate.
- DLQ: lưu message xử lý lỗi sau max receives.
- Long polling: giảm empty responses và cost. Long polling tốt hơn short polling cho worker pattern.
- Standard queue: high throughput, at-least-once, best-effort ordering.
- FIFO queue: ordering theo message group, exactly-once deduplication, throughput giới hạn hơn Standard.
- Delay queue/message timer: trì hoãn message.

Chọn SQS khi:

- Producer và consumer cần decouple.
- Worker cần pull job theo tốc độ xử lý.
- Cần buffer traffic spike.
- Cần retry/DLQ.

## SNS High-Yield

- Topic push message đến subscriber: SQS, Lambda, HTTP/S, email, SMS, mobile push.
- Fan-out pattern: SNS topic -> nhiều SQS queues -> mỗi consumer xử lý riêng.
- Message filtering giúp subscriber nhận subset event.

Chọn SNS khi:

- Một event cần thông báo nhiều hệ thống.
- Alert/notification.
- Pub/sub đơn giản.

## EventBridge High-Yield

- Event bus cho AWS events, SaaS events, custom app events.
- Rule match pattern và route tới target.
- Scheduler cho cron/rate schedule.
- Archive/replay event trong một số use case.

Chọn EventBridge khi:

- Cần route event theo rule.
- Tích hợp SaaS/AWS service events.
- Xây event-driven architecture loosely coupled.

## Step Functions

| Workflow type | Dùng khi |
|---|---|
| Standard | Long-running, exactly-once workflow execution, audit history |
| Express | High-volume, short-duration workflow, cost thấp hơn cho event volume lớn |

Capabilities:

- Retry/catch.
- Choice/parallel/map state.
- Human approval pattern.
- Visual workflow.

Chọn Step Functions khi đề mô tả nhiều bước có retry/branching/orchestration.

## API Gateway And AppSync

API Gateway:

- REST API: feature-rich, API keys, usage plans.
- HTTP API: low latency/cost, simpler.
- WebSocket API: bidirectional communication.
- Throttling, authorization, caching, custom domain.

AppSync:

- GraphQL API.
- Resolvers tới DynamoDB/Lambda/OpenSearch/HTTP.
- Realtime subscriptions.

## Observability

| Service | Dùng khi |
|---|---|
| CloudWatch Metrics | CPU, latency, custom metric, alarms |
| CloudWatch Logs | Central logs |
| CloudWatch Alarms | Trigger notification/scaling |
| CloudWatch Synthetics | Canary monitor |
| CloudWatch RUM | Real user monitoring |
| X-Ray | Distributed tracing |
| CloudTrail | API audit |
| AWS Config | Resource config history/compliance |

High-yield:

- EC2 memory/disk metrics không mặc định; cần CloudWatch Agent.
- CloudTrail management events mặc định cho account history; trail gửi log tới S3/CloudWatch Logs.
- Config trả lời "resource từng được cấu hình như thế nào" và compliance rule.
- X-Ray giúp trace request qua microservices.

## Governance And Operations

| Service | Dùng khi |
|---|---|
| CloudFormation | Infrastructure as Code |
| Systems Manager | Patch, Session Manager, Run Command, Parameter Store |
| Trusted Advisor | Best practice checks |
| Compute Optimizer | Rightsizing EC2/EBS/Lambda/ECS recommendations |
| Service Catalog | Approved products/templates cho organization |
| Well-Architected Tool | Review workload theo 6 pillar |
| Health Dashboard | AWS events ảnh hưởng account/service |
| License Manager | License tracking |

## Cost Management

| Tool | Dùng khi |
|---|---|
| Cost Explorer | Phân tích spend theo thời gian/service/tag |
| Budgets | Cảnh báo budget/usage/reservation/savings plans |
| Cost and Usage Report | Dữ liệu billing chi tiết nhất |
| Cost allocation tags | Gắn cost theo team/project/env |
| Savings Plans | Cam kết usage spend cho compute |
| Reserved Instances | Cam kết instance/db/cache capacity |
| Compute Optimizer | Rightsizing recommendation |
| S3 Lifecycle/Intelligent-Tiering | Tối ưu storage cost |

Cost traps:

- NAT Gateway data processing cost.
- Cross-AZ and cross-Region data transfer.
- Overprovisioned RDS/EC2.
- S3 Intelligent-Tiering: object nhỏ hơn **128KB** không được automatic tiering nhưng vẫn có monitoring fee; không hiệu quả cho object nhỏ.
- S3 Standard-IA/One Zone-IA có minimum duration 30 ngày và minimum billable object size 128KB.
- Read replicas/Multi-AZ tăng cost nhưng phục vụ mục tiêu khác nhau.

## Exam Decision Rules

| Requirement | Chọn |
|---|---|
| Decouple producer/consumer | SQS |
| Fan-out one event to many consumers | SNS -> SQS queues |
| Rule-based event routing/SaaS events | EventBridge |
| Multi-step workflow with retry/branching | Step Functions |
| REST API to Lambda | API Gateway |
| GraphQL/realtime | AppSync |
| Audit who did what | CloudTrail |
| Monitor metrics/logs/alarm | CloudWatch |
| Trace distributed request | X-Ray |
| Resource compliance/history | Config |
| Patch/manage EC2 without SSH | Systems Manager |
| Analyze cost trends | Cost Explorer |
| Alert when spend exceeds threshold | Budgets |
| Detailed billing data | Cost and Usage Report |

## Liên Kết

- [Compute](compute.md)
- [Databases](databases.md)
- [Service Selection Matrix](../02-architecture/service-selection-matrix.md)

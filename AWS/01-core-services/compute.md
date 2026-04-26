# Compute

Compute trong SAA-C03 không chỉ là "chạy server". Đề thường hỏi chọn compute theo operational overhead, scaling, latency, cost, workload duration, và mức kiểm soát OS.

## Service Map

| Service | Dùng khi | Không hợp khi | Bẫy đề |
|---|---|---|---|
| EC2 | Cần kiểm soát OS, legacy app, custom runtime, long-running service | Muốn zero server management | EC2 trong private subnet không tự ra internet nếu không có NAT hoặc endpoint |
| EC2 Auto Scaling | Cần scale ngang theo metric/schedule | Stateful app không tách state | ASG tăng availability khi chạy nhiều AZ |
| Lambda | Event-driven, request ngắn, scale tự động, trả tiền theo thời gian chạy | Task dài hơn 15 phút, cần full OS control | Lambda trong VPC cần route/endpoint/NAT để ra dịch vụ ngoài VPC |
| ECS | Chạy container AWS-native | Muốn Kubernetes API | ECS có launch type EC2 hoặc Fargate |
| Fargate | Chạy container không quản lý EC2 | Cần daemon/host-level customization sâu | Fargate giảm operational overhead |
| EKS | Cần Kubernetes ecosystem/API | Chỉ cần container đơn giản | SAA hỏi EKS ít hơn ECS/Fargate nhưng vẫn in-scope |
| ECR | Registry lưu Docker image | Không chạy container | Kết hợp ECS/EKS/Lambda container image |
| Batch | Batch job, queue job, compute intensive | API request realtime | Có thể dùng Spot để giảm cost |
| Elastic Beanstalk | Deploy app nhanh, AWS quản lý infra cơ bản | Cần IaC/kiểm soát chi tiết | Beanstalk không phải serverless |
| Outposts / Wavelength | Low latency on-prem/telco edge | Workload cloud bình thường | Dùng khi requirement locality/latency rõ |

Nguồn: [AWS EC2](https://docs.aws.amazon.com/ec2/), [AWS Lambda](https://docs.aws.amazon.com/lambda/), [Amazon ECS](https://docs.aws.amazon.com/ecs/), [AWS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html).

## EC2 High-Yield

### Instance Families

| Family | Nghĩa | Use case |
|---|---|---|
| T | Burstable general purpose | dev/test, low traffic, burst ngắn |
| M | General purpose | web/app server cân bằng |
| C | Compute optimized | CPU-bound, batch, gaming, high-performance web |
| R/X | Memory optimized | cache, in-memory DB, analytics memory-heavy |
| I/D/H | Storage optimized | local NVMe/HDD, high I/O, big data |
| G/P/Inf/Trn | Accelerated computing | GPU/ML/HPC |

### Purchasing Options

| Option | Dùng khi | Ghi nhớ |
|---|---|---|
| On-Demand | Không cam kết, workload ngắn/khó đoán | Linh hoạt nhất, cost cao hơn |
| Savings Plans | Cam kết spend theo giờ 1 hoặc 3 năm | Linh hoạt hơn Reserved Instances |
| Reserved Instances | Workload ổn định, predictable | Standard RI ít linh hoạt, Convertible RI linh hoạt hơn |
| Spot Instances | Fault-tolerant, interruptible job | Có thể bị reclaim, phù hợp batch/worker/stateless |
| Dedicated Hosts | Compliance/license per host | Đắt, dùng khi license hoặc isolation yêu cầu |

### Placement Groups

| Type | Dùng khi | Trade-off |
|---|---|---|
| Cluster | Low latency/high throughput trong một AZ | Ít fault tolerance hơn spread |
| Spread | Mỗi instance trên hardware khác nhau | Giới hạn số instance mỗi AZ |
| Partition | Big distributed system như Hadoop/Cassandra/Kafka | Fault isolation theo partition |

### Auto Scaling

- Target tracking: giữ metric ở mức mục tiêu, ví dụ CPU 50%.
- Step scaling: tăng/giảm theo bậc khi alarm vượt ngưỡng.
- Scheduled scaling: biết trước peak.
- Predictive scaling: AWS dự đoán pattern.
- Health check: EC2 status check hoặc ELB health check.

Bẫy đề:

- Muốn high availability: ASG phải trải nhiều AZ sau ALB.
- Muốn scale theo queue depth: dùng CloudWatch metric từ SQS để scale worker.
- Muốn không mất session khi scale: session externalize vào ElastiCache/DynamoDB, không giữ trong EC2 local memory.

## Load Balancing

| Load Balancer | Layer | Dùng khi |
|---|---:|---|
| ALB | Layer 7 | HTTP/HTTPS, path/host-based routing, container, Lambda target |
| NLB | Layer 4 | TCP/UDP/TLS, cực thấp latency, static IP, high throughput |
| Gateway Load Balancer | Layer 3/4 appliance flow | Deploy firewall/IDS/IPS appliance transparently |
| Classic Load Balancer | Legacy | Tránh chọn nếu có ALB/NLB phù hợp |

## Lambda High-Yield

### Nên Dùng

- S3 event xử lý ảnh/file.
- API Gateway + Lambda cho API serverless.
- EventBridge schedule hoặc event bus.
- DynamoDB Streams trigger.
- SQS consumer.

### Không Nên Dùng

- Job chạy quá 15 phút.
- App cần full OS/kernel control.
- App có connection pooling phức tạp mà không dùng RDS Proxy/thiết kế phù hợp.

### Điểm Thi Hay Hỏi

- Timeout tối đa 15 phút.
- Memory tăng thì CPU cũng tăng theo.
- Cold start có thể giảm bằng Provisioned Concurrency.
- Lambda concurrency có giới hạn theo account/Region và có thể reserve per function.
- Lambda trong VPC truy cập private resources qua ENI; muốn gọi internet cần NAT Gateway hoặc endpoint phù hợp.
- Với RDS, dùng RDS Proxy để quản lý connection pool và giảm áp lực DB.

## Containers

### ECS

- Cluster: nhóm tài nguyên chạy task/service.
- Task definition: blueprint container.
- Service: duy trì số task mong muốn.
- Launch type:
  - EC2: tự quản lý EC2 worker, tối ưu cost/control.
  - Fargate: không quản lý server, giảm vận hành.

### EKS

- Managed Kubernetes control plane.
- Dùng khi tổ chức đã chuẩn hóa Kubernetes, cần Helm/operator/K8s API.
- Compute có thể là managed node groups, self-managed nodes, hoặc Fargate.

### ECR

- Private Docker registry trên AWS.
- Tích hợp IAM, image scanning, lifecycle policy.

## Batch, Beanstalk, Edge Compute

### AWS Batch

Chọn khi workload là queue of jobs, có thể retry, cần compute environment scale theo hàng đợi. Kết hợp Spot để tiết kiệm nếu job chịu interrupt.

### Elastic Beanstalk

Chọn khi muốn deploy app truyền thống nhanh mà vẫn dùng EC2/ALB/ASG/RDS bên dưới. Đề thường chọn Beanstalk khi yêu cầu "developer wants to deploy without managing infrastructure" nhưng vẫn là app web truyền thống.

### Outposts / Wavelength

- Outposts: AWS hardware đặt tại on-premises để đáp ứng latency/data residency.
- Wavelength: AWS compute/storage ở telco edge cho mobile/5G low latency.

## Exam Decision Rules

| Requirement | Chọn |
|---|---|
| Full OS control | EC2 |
| Event-driven, ngắn, serverless | Lambda |
| Container, ít vận hành | ECS/Fargate |
| Kubernetes | EKS |
| Batch job scale theo queue | AWS Batch |
| Web app deploy nhanh, ít infra config | Elastic Beanstalk |
| Fault-tolerant worker rẻ | EC2 Spot trong ASG hoặc Batch Spot |
| Stable baseline workload | Savings Plans hoặc RI |
| Need static IP/high TCP throughput | NLB |
| HTTP path routing | ALB |

## Liên Kết

- [Storage](storage.md)
- [Networking](networking.md)
- [Databases](databases.md)
- [High-Yield Comparisons](../02-architecture/high-yield-comparisons.md)

# AWS Handbook - SAA-C03 (AWS Certified Solutions Architect – Associate)

Tài liệu này tổng hợp kiến thức về các dịch vụ AWS cốt lõi phục vụ cho kỳ thi AWS SAA-C03. Nội dung tập trung vào: **Là gì?**, **Dùng để làm gì?**, và **Khi nào nên dùng?**.

---

## 1. Compute (Tính toán)

Đây là xương sống của mọi ứng dụng. AWS cung cấp nhiều loại hình compute từ máy ảo thô sơ đến serverless hiện đại.

### 1.1 EC2 (Elastic Compute Cloud)
*   **Là gì?**: Máy chủ ảo (Virtual Server) trên cloud. Bạn có toàn quyền kiểm soát OS (Linux/Windows), CPU, RAM.
*   **Dùng để làm gì?**: Chạy ứng dụng web, database, backend server, worker process... bất cứ thứ gì chạy được trên máy tính.
*   **Khi nào dùng?**:
    *   Khi bạn cần toàn quyền kiểm soát hệ điều hành và môi trường.
    *   Khi ứng dụng cũ (legacy) khó chuyển sang container/serverless.
    *   Cần Reserved Instance để tiết kiệm chi phí cho các workload chạy liên tục 24/7.
    *   **Các loại Instance chính (Instance Types)**:
        *   **General Purpose (T3, M5)**: Cân bằng, dùng cho web server, code repo.
        *   **Compute Optimized (C5)**: Dùng cho tính toán nặng (Batch processing, Gaming server).
        *   **Memory Optimized (R5)**: Dùng cho RAM lớn (Database, Cache).
    *   **Tùy chọn mua (Purchasing Options)**:
        *   **On-Demand**: Trả tiền theo giờ/giây. Đắt nhất, linh hoạt nhất.
        *   **Reserved (RI)**: Cam kết 1-3 năm. Rẻ hơn tới 72% so với On-Demand.
        *   **Spot Instance**: Đấu giá "hàng ế" của AWS. Rẻ hơn tới 90%, nhưng có thể bị lấy lại bất cứ lúc nào (báo trước 2 phút). Dùng cho job có thể bị gián đoạn.

### 1.2 Lambda
*   **Là gì?**: Dịch vụ Serverless Compute. Bạn chỉ viết code (Hàm), AWS lo việc chạy server, scale. Bạn trả tiền theo mili-giây code chạy.
*   **Dùng để làm gì?**: Xử lý sự kiện (event-driven), API backend (kết hợp API Gateway), xử lý file S3 tự động, cron job.
*   **Khi nào dùng?**:
    *   Các tác vụ ngắn (< 15 phút).
    *   Các tác vụ không chạy liên tục (tiết kiệm tiền hơn EC2).
    *   Xử lý sự kiện thời gian thực.
    *   **Lưu ý quan trọng**:
        *   **Thời gian chạy tối đa**: 15 phút.
        *   **Cold Start**: Lần chạy đầu tiên sẽ chậm một chút do AWS phải khởi tạo môi trường.
        *   **Memory**: Cấu hình từ 128MB đến 10GB (CPU scale theo Memory).

### 1.3 ECS (Elastic Container Service)
*   **Là gì?**: Dịch vụ quản lý Docker Container. Giúp bạn chạy và quản lý hàng cụm container.
*   **Dùng để làm gì?**: Chạy các ứng dụng đóng gói Docker (Microservices).
*   **Khi nào dùng?**: Khi bạn đã dùng Docker và muốn AWS quản lý việc orchestration (triển khai, scale) thay vì tự cài Kubernetes.

### 1.4 Fargate
*   **Là gì?**: Serverless Engine cho ECS (và EKS). Là "cách chạy" container mà không cần quản lý EC2 bên dưới.
*   **Dùng để làm gì?**: Chạy container mà không muốn lo về việc patch OS, quản lý cụm server EC2.
*   **Khi nào dùng?**: Muốn chạy Docker đơn giản, rảnh tay quản lý hạ tầng (tuy nhiên giá có thể cao hơn EC2 một chút nếu tối ưu không khéo).

---

## 2. Storage (Lưu trữ)

Dữ liệu là vàng. AWS có các kho chứa khác nhau tùy nhu cầu.

### 2.1 S3 (Simple Storage Service)
*   **Là gì?**: Kho lưu trữ Object (Object Storage). Lưu file dạng phẳng (flat), không có thư mục thực sự (chỉ là prefix).
*   **Dùng để làm gì?**: Lưu ảnh, video, log, backup, static website (HTML/CSS/JS).
*   **Khi nào dùng?**:
    *   Lưu trữ dữ liệu phi cấu trúc (Unstructured data).
    *   Lưu trữ lâu dài, giá rẻ (S3 Glacier).
    *   Static Website Hosting.
    *   **Các lớp lưu trữ (Storage Classes)**:
        *   **Standard**: Truy cập thường xuyên, đắt nhất, độ trễ thấp nhất.
        *   **Standard-IA (Infrequent Access)**: Ít truy cập nhưng cần lấy ngay. Rẻ hơn Standard.
        *   **Intelligent-Tiering**: Tự động chuyển đổi giữa Standard và IA dựa trên thói quen sử dụng (Dùng khi không đoán được pattern truy cập).
        *   **Glacier / Deep Archive**: Lưu trữ lâu dài (Backup), rẻ như cho. Lấy lại dữ liệu mất từ vài phút đến 12 giờ.

### 2.2 EBS (Elastic Block Store)
*   **Là gì?**: Ổ cứng ảo gắn vào EC2. (Block Storage).
*   **Dùng để làm gì?**: Cài hệ điều hành, cài Database (MySQL, PostgreSQL) chạy trên EC2.
*   **Khi nào dùng?**:
    *   Cần ổ cứng hiệu năng cao, độ trễ thấp cho EC2.
    *   Dữ liệu cần persistence (tồn tại) khi EC2 stop (nhưng EBS gắn chặt với 1 Availability Zone - AZ).
    *   **Các loại Volume chính**:
        *   **gp2 / gp3 (General Purpose SSD)**: Cân bằng giá/hiệu năng. Dùng cho Boot volume, Dev/Test.
        *   **io1 / io2 (Provisioned IOPS SSD)**: Hiệu năng cực cao (Database quan trọng). Đắt tiền.
        *   **st1 / sc1 (HDD)**: Dùng cho Big Data, Log processing (Throughput cao nhưng IOPS thấp). Rẻ.

### 2.3 EFS (Elastic File System)
*   **Là gì?**: Ổ đĩa mạng (Network File System - NFS).
*   **Dùng để làm gì?**: Chia sẻ file giữa nhiều EC2 instance cùng lúc.
*   **Khi nào dùng?**:
    *   Khi nhiều server cần đọc/ghi vào cùng 1 thư mục (VD: CMS, code sharing).
    *   Cần file system dạng Linux chuẩn (POSIX complaint) mà S3 không đáp ứng được.

---

## 3. Database (Cơ sở dữ liệu)

### 3.1 RDS (Relational Database Service)
*   **Là gì?**: Dịch vụ Database quan hệ được quản lý (Managed SQL). Hỗ trợ: MySQL, PostgreSQL, MariaDB, Oracle, SQL Server.
*   **Dùng để làm gì?**: Lưu dữ liệu có cấu trúc, quan hệ chặt chẽ (đơn hàng, user).
*   **Khi nào dùng?**:
    *   Cần SQL truyền thống.
    *   Muốn AWS lo việc backup, patch, Multi-AZ (dự phòng).
    *   **Tính năng chính**:
        *   **Multi-AZ**: Đồng bộ dữ liệu sang AZ khác để dự phòng (Disaster Recovery). Nếu DB chính chết, DB phụ tự lên thay.
        *   **Read Replica**: Tạo bản sao chỉ đọc (Read-only) để giảm tải cho DB chính. Dữ liệu được sync bất đồng bộ (có độ trễ nhỏ).

### 3.2 Aurora
*   **Là gì?**: Phiên bản RDS "độ" của AWS. Tương thích MySQL/PostgreSQL nhưng nhanh hơn gấp 5 lần.
*   **Dùng để làm gì?**: Các hệ thống Enterprise cần hiệu năng cực cao và tính sẵn sàng cao (High Availability).
*   **Khi nào dùng?**: Khi RDS thường bị quá tải hoặc cần tính năng Serverless (Aurora Serverless) tự động scale theo tải.

### 3.3 DynamoDB
*   **Là gì?**: Database NoSQL (Key-Value) serverless.
*   **Dùng để làm gì?**: Lưu dữ liệu dạng document/key-value, cần tốc độ phản hồi cực nhanh (mili-giây đơn vị) ở mọi quy mô. Giỏ hàng, Session, Game score.
*   **Khi nào dùng?**:
    *   Schema không cố định.
    *   Tải cực lớn (hàng triệu request/giây).
    *   Cần Serverless DB (không cần quản lý connection).
    *   **Tính năng chính**:
        *   **DAX (Accelerator)**: Caching layer dành riêng cho DynamoDB, giúp giảm độ trễ từ mili-giây xuống micro-giây.
        *   **Global Tables**: Tự động replicate dữ liệu sang nhiều Region khác nhau (Multi-Region Active-Active).
        *   **RCU/WCU**: Đơn vị tính tiền dựa trên khả năng Đọc/Ghi (Read/Write Capacity Unit). Có chế độ **On-Demand** (tự scale theo tải) hoặc **Provisioned** (cài cứng số lượng để tiết kiệm).

### 3.4 ElastiCache
*   **Là gì?**: Caching in-memory (Redis / Memcached).
*   **Dùng để làm gì?**: Lưu cache để giảm tải cho Database chính.
*   **Khi nào dùng?**:
    *   Ứng dụng đọc nhiều (Read-heavy).
    *   Cần độ trễ thấp (micro-seconds).
    *   Lưu Session người dùng, bảng xếp hạng (Redis Sorted Sets).

---

## 4. Networking (Mạng)

### 4.1 VPC (Virtual Private Cloud)
*   **Là gì?**: Mạng riêng ảo của bạn trên AWS. Một khu đất riêng biệt bị rào lại.
*   **Dùng để làm gì?**: Chứa tất cả EC2, RDS, Lambda... để bảo mật.
*   **Khi nào dùng?**: Luôn luôn dùng khi khởi tạo hạ tầng.
    *   **Thành phần con**:
        *   **Subnet**: Chia nhỏ mạng. **Public Subnet** (ra được Internet), **Private Subnet** (không ra được Internet).
        *   **NAT Gateway**: Giúp EC2 trong Private Subnet đi ra Internet để update (nhưng Internet không chui ngược vào được).
        *   **Security Group**: Firewall ở cấp độ Instance (Stateful). Chỉ cần mở Inbound, Outbound tự mở.
        *   **NACL**: Firewall ở cấp độ Subnet (Stateless). Phải mở cả Inbound và Outbound.

### 4.2 Route 53
*   **Là gì?**: Dịch vụ DNS (Domain Name System).
*   **Dùng để làm gì?**: Đăng ký tên miền (VD: `example.com`), trỏ tên miền về IP server.
*   **Khi nào dùng?**:
    *   Quản lý DNS.
    *   Health Check & Routing (điều hướng người dùng đến server gần nhất hoặc server còn sống).
    *   **Các loại Routing Policies**:
        *   **Simple**: Trỏ thẳng IP, không check gì cả.
        *   **Weighted**: Chia tải theo % (VD: 80% về Server A, 20% về Server B để test tính năng mới).
        *   **Latency**: Điều hướng user về Region có độ trễ thấp nhất.
        *   **Failover**: Nếu Server chính chết (Health check fail), tự động trỏ về Server dự phòng.

### 4.3 CloudFront
*   **Là gì?**: CDN (Content Delivery Network). Hệ thống server đệm trên toàn cầu (Edge Location).
*   **Dùng để làm gì?**: Cache nội dung (ảnh/video) tại vị trí gần người dùng nhất để tải nhanh hơn.
*   **Khi nào dùng?**:
    *   Website phục vụ khách toàn cầu.
    *   Giảm tải cho S3/EC2 gốc.
    *   Chống DDoS cơ bản.

---

## 5. Security & Identity (Bảo mật)

### 5.1 IAM (Identity and Access Management)
*   **Là gì?**: Quản lý User, Group, Role và Quyền hạn (Permission).
*   **Dùng để làm gì?**: Kiểm soát "Ai được làm gì" trên AWS Account của bạn.
*   **Khi nào dùng?**: Bước đầu tiên khi setup account. **Nguyên tắc**: Least Privilege (chỉ cấp quyền tối thiểu cần thiết).

### 5.2 KMS (Key Management Service)
*   **Là gì?**: Dịch vụ quản lý khóa mã hóa.
*   **Dùng để làm gì?**: Tạo và quản lý key để mã hóa dữ liệu trong EBS, S3, RDS.
*   **Khi nào dùng?**: Khi cần bảo mật dữ liệu (Encryption at rest).

### 5.3 WAF (Web Application Firewall)
*   **Là gì?**: Tường lửa ứng dụng web.
*   **Dùng để làm gì?**: Chặn các cuộc tấn công web phổ biến (SQL Injection, XSS) vào CloudFront hoặc ALB.
*   **Khi nào dùng?**: Bảo vệ ứng dụng web ở lớp 7 (Application Layer).

---

## 6. Application Integration (Tích hợp ứng dụng)

### 6.1 SQS (Simple Queue Service)
*   **Là gì?**: Hàng đợi tin nhắn (Message Queue).
*   **Dùng để làm gì?**: Giúp các service giao tiếp bất đồng bộ (Decoupling). Service A gửi tin nhắn vào SQS, Service B rảnh thì lấy ra xử lý.
*   **Khi nào dùng?**:
    *   Xử lý background job (gửi mail, resize ảnh).
    *   Đảm bảo không mất tin nhắn nếu worker chết.

### 6.2 SNS (Simple Notification Service)
*   **Là gì?**: Dịch vụ thông báo (Pub/Sub).
*   **Dùng để làm gì?**: Gửi thông báo đến nhiều nơi cùng lúc (Fan-out): Email, SMS, Lambda, SQS.
*   **Khi nào dùng?**: Gửi cảnh báo hệ thống, push notification.

### 6.3 Step Functions
*   **Là gì?**: Máy trạng thái (State Machine) serverless.
*   **Dùng để làm gì?**: Điều phối quy trình nghiệp vụ phức tạp gồm nhiều bước Lambda (Workflow orchestration).
*   **Khi nào dùng?**:
    *   Quy trình order: Thanh toán -> Trừ kho -> Gửi mail.
    *   Thay thế việc gọi Lambda lồng nhau (Lambda gọi Lambda) rất khó debug.

---

## 7. Management & Governance

### 7.1 CloudWatch
*   **Là gì?**: Dịch vụ giám sát (Monitoring).
*   **Dùng để làm gì?**: Thu thập Log, Metrics (CPU, Memory), Set Alarm (Cảnh báo).
*   **Khi nào dùng?**: Theo dõi sức khỏe hệ thống, debug lỗi.

### 7.2 CloudTrail
*   **Là gì?**: Camera giám sát (Auditing).
*   **Dùng để làm gì?**: Ghi lại "Ai đã làm gì, vào lúc nào?". Mọi API call đều được lưu lại.
*   **Khi nào dùng?**:
    *   Kiểm tra bảo mật (Audit).
    *   Truy tìm thủ phạm xóa nhầm resource.

---

## 8. AWS Global Infrastructure (Cơ sở hạ tầng toàn cầu)

### 8.1 Region (Khu vực)
*   **Là gì?**: Một vị trí địa lý vật lý (VD: us-east-1, ap-southeast-1). Chứa nhiều Availability Zone.
*   **Nguyên tắc**: Luôn chọn Region gần khách hàng nhất để giảm độ trễ (Latency).

### 8.2 Availability Zone (AZ - Vùng sẵn sàng)
*   **Là gì?**: Một hoặc nhiều Data Center tách biệt về điện, mạng, làm mát.
*   **Khi nào dùng?**: Luôn triển khai ứng dụng trên ít nhất **2 AZ** trở lên (Multi-AZ) để đảm bảo High Availability (HA). Nếu 1 AZ bị thiên tai, ứng dụng vẫn chạy ở AZ kia.

### 8.3 Edge Location
*   **Là gì?**: Trạm trung chuyển dữ liệu (nhiều hơn Region rất nhiều).
*   **Dùng để làm gì?**: Được CloudFront dùng để cache nội dung.

---

## 9. Migration & Transfer (Di chuyển dữ liệu)

### 9.1 Snow Family (Snowball, Snowmobile)
*   **Là gì?**: Thiết bị lưu trữ vật lý (Vali ổ cứng).
*   **Dùng để làm gì?**: Chuyển dữ liệu cực lớn (Petabytes) từ On-premise lên AWS bằng đường... chuyển phát nhanh (xe tải) thay vì qua mạng Internet.
*   **Khi nào dùng?**: Khi việc upload qua mạng mất quá 1 tuần.

### 9.2 DataSync
*   **Là gì?**: Đồng bộ dữ liệu tự động qua mạng.
*   **Dùng để làm gì?**: Sync file từ Server cũ lên S3/EFS liên tục.

---

## 10. Cost Management (Quản lý chi phí)

### 10.1 AWS Budgets
*   **Dùng để làm gì?**: Đặt ngân sách (VD: $50/tháng). Nếu dùng lố, nó sẽ gửi mail cảnh báo.
*   **Khi nào dùng?**: Luôn cài đặt ngay lập tức để tránh bị "bill shock".

### 10.2 Cost Explorer
*   **Dùng để làm gì?**: Xem biểu đồ chi tiêu, phân tích tiền đi đâu.

---

## 11. AWS Well-Architected Framework (6 Trụ cột)

Để thi đỗ SAA-C03, bạn **phải thuộc lòng** 6 trụ cột này khi thiết kế hệ thống:

1.  **Operational Excellence (Vận hành xuất sắc)**: Tự động hóa, theo dõi, cải tiến liên tục (Sử dụng CloudFormation, CloudWatch).
2.  **Security (Bảo mật)**: Bảo vệ dữ liệu, quản lý quyền hạn (IAM, KMS, WAF).
3.  **Reliability (Độ tin cậy)**: Hệ thống tự phục hồi khi có lỗi (Auto Scaling, Multi-AZ).
4.  **Performance Efficiency (Hiệu năng)**: Sử dụng đúng loại tài nguyên, serverless (Lambda, Aurora).
5.  **Cost Optimization (Tối ưu chi phí)**: Chỉ trả tiền cho những gì sử dụng, dùng đúng loại EC2 (Spot Instance, S3 Intelligent-Tiering).
6.  **Sustainability (Bền vững)**: Giảm thiểu tác động môi trường, tối ưu tài nguyên phần cứng.

---

## 12. Tài liệu tham khảo & Kiến trúc mẫu

Để hiểu rõ hơn cách kết hợp các dịch vụ trên thành một hệ thống thực tế (3-Tier, Serverless, v.v.), hãy xem tài liệu đi kèm:

👉 **[AWS Architecture Patterns (Common Combos)](aws_architecture_combos.md)**


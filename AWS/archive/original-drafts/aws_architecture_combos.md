# AWS Architecture Patterns (Common Combos)

Tài liệu này tổng hợp các mô hình kiến trúc phổ biến nhất trên AWS mà bạn sẽ gặp thường xuyên trong thực tế và các kỳ thi chứng chỉ.

---

## 1. 3-Tier Web Architecture (Kiến trúc Web 3 Lớp cổ điển)

Đây là "nền tảng" của hầu hết các ứng dụng doanh nghiệp.

### Mô hình
```mermaid
flowchart TD
    %% Styles
    classDef client fill:#4FC3F7,stroke:#0277BD,stroke-width:2px,color:white;
    classDef lb fill:#FFCA28,stroke:#FFA000,stroke-width:2px,color:black;
    classDef compute fill:#FF9800,stroke:#E65100,stroke-width:2px,color:white;
    classDef db fill:#BA68C8,stroke:#6A1B9A,stroke-width:2px,color:white,shape:cylinder;
    classDef net fill:#455A64,stroke:#37474F,stroke-width:2px,stroke-dasharray: 5 5,color:white;

    subgraph VPC ["VPC Cloud"]
        direction TB
        
        subgraph PublicSubnet ["Public Subnet (DMZ)"]
            ALB["Application Load Balancer"]:::lb
            NAT["NAT Gateway"]:::net
        end

        subgraph PrivateWeb ["Private Subnet (App Layer)"]
             EC2_1["EC2 - Web Server 1"]:::compute
             EC2_2["EC2 - Web Server 2"]:::compute
             ASG["Auto Scaling Group"]:::net
        end

        subgraph PrivateData ["Private Subnet (Data Layer)"]
            MasterDB[("RDS Master")]:::db
            SlaveDB[("RDS Standby")]:::db
        end
    end

    User((User)):::client --> Internet
    Internet --> ALB

    ALB --> EC2_1 & EC2_2
    EC2_1 & EC2_2 --> MasterDB
    MasterDB -. "Replication" .-> SlaveDB
    EC2_1 -. "Outbound Internet" .-> NAT
```

### Chi tiết
1.  **Public Subnet**: Chứa **Load Balancer (ALB)** và **NAT Gateway**. Đây là nơi duy nhất tiếp xúc trực tiếp với Internet.
2.  **Private Subnet (App)**: Chứa các **EC2 Instance**. Các server này **KHÔNG** có Public IP. Chúng chỉ nhận request từ ALB.
3.  **Private Subnet (Data)**: Chứa **RDS Database**. Được bảo vệ kỹ nhất, chỉ cho phép EC2 ở lớp App truy cập (qua Security Group).

### Khi nào dùng?
*   Hệ thống Enterprise, E-commerce cần bảo mật cao.
*   Cần sự ổn định và kiểm soát server truyền thống.
*   Dễ dàng Audit và tuân thủ các chuẩn bảo mật (PCI-DSS).

---

## 2. Serverless Architecture (Kiến trúc Hiện đại)

Mô hình này giúp bạn quên đi nỗi lo quản lý server. "Pay-as-you-go" (Dùng bao nhiêu trả bấy nhiêu).

### Mô hình
```mermaid
flowchart LR
    %% Styles
    classDef client fill:#4FC3F7,stroke:#0277BD,stroke-width:2px,color:white;
    classDef api fill:#FFCA28,stroke:#FFA000,stroke-width:2px,color:black;
    classDef compute fill:#FF9800,stroke:#E65100,stroke-width:2px,color:white;
    classDef db fill:#BA68C8,stroke:#6A1B9A,stroke-width:2px,color:white,shape:cylinder;

    User((User)):::client --> API_GW["API Gateway"]:::api
    API_GW --> Lambda["AWS Lambda\n(Business Logic)"]:::compute
    Lambda --> DynamoDB[("DynamoDB\n(NoSQL Data)")]:::db
```

### Chi tiết
1.  **API Gateway**: Nhận HTTP Request từ Client, đóng vai trò như cửa ngõ, có thể Throttle (giới hạn tốc độ) và Auth.
2.  **Lambda**: Chạy code xử lý logic khi có request. Tự động scale từ 0 lên 10.000 request/giây mà không cần cấu hình.
3.  **DynamoDB**: Lưu trữ dữ liệu dạng Key-Value cực nhanh. Tự động scale.

### Khi nào dùng?
*   Startup, MVP (Sản phẩm demo) cần ra mắt nhanh.
*   Ứng dụng có lượng truy cập biến động mạnh (lúc vắng tanh, lúc bùng nổ).
*   API Restful đơn giản.

---

## 3. High Performance Static Web (S3 + CloudFront)

Cách rẻ nhất và nhanh nhất để host một trang web frontend (React, Angular, Vue).

### Mô hình
```mermaid
flowchart LR
    %% Styles
    classDef client fill:#4FC3F7,stroke:#0277BD,stroke-width:2px,color:white;
    classDef cdn fill:#E91E63,stroke:#880E4F,stroke-width:2px,color:white;
    classDef storage fill:#8BC34A,stroke:#33691E,stroke-width:2px,color:white,shape:cylinder;
    classDef edge fill:#F5F5F5,stroke:#9E9E9E,stroke-width:1px,stroke-dasharray: 5 5;

    subgraph Edge ["Edge Location (Toàn cầu)"]
        CF["CloudFront (CDN)"]:::cdn
    end

    User((User)):::client --> CF
    CF -. "Cache Miss" .-> S3[("S3 Bucket\n(Origin)")]:::storage
    CF -- "Cache Hit (Fast)" --> User
```

### Chi tiết
1.  **S3 Bucket**: Chứa file code web (HTML, CSS, JS, Images). S3 được cấu hình làm **Static Website Hosting**.
2.  **CloudFront**: Phân phối nội dung từ S3 ra các Edge Location trên toàn thế giới.
3.  **Lợi ích**: Người dùng ở Việt Nam sẽ tải web từ server CloudFront ở Việt Nam/Singapore thay vì tải từ S3 tận bên Mỹ -> Siêu nhanh.

### Khi nào dùng?
*   Web tin tức, Blog, Landing Page.
*   Single Page Application (SPA) viết bằng React/Vue/Angular.

---

## 4. Fan-out Pattern (SNS + SQS)

Mô hình xử lý song song, giúp một sự kiện kích hoạt nhiều hành động khác nhau.

### Mô hình
```mermaid
flowchart LR
    %% Styles
    classDef event fill:#FF9800,stroke:#E65100,stroke-width:2px,color:white;
    classDef topic fill:#FF5722,stroke:#BF360C,stroke-width:2px,color:white;
    classDef queue fill:#2196F3,stroke:#0D47A1,stroke-width:2px,color:white;
    classDef worker fill:#607D8B,stroke:#263238,stroke-width:2px,color:white;

    Event["Order Created"]:::event --> SNS["SNS Topic"]:::topic
    
    SNS --> SQS1["SQS: Email Queue"]:::queue
    SNS --> SQS2["SQS: Analytics Queue"]:::queue
    SNS --> SQS3["SQS: Inventory Queue"]:::queue

    SQS1 --> Worker1["Lambda: Gửi Mail"]:::worker
    SQS2 --> Worker2["Lambda: Lưu Data"]:::worker
    SQS3 --> Worker3["EC2: Trừ kho"]:::worker
```

### Chi tiết
1.  **SNS Topic**: Nhận sự kiện gốc ("Có đơn hàng mới!").
2.  **SQS Queues**: Nhiều hàng đợi cùng đăng ký (Subscribe) vào 1 Topic. Khi SNS có tin, nó copy tin đó ném vào TẤT CẢ các Queue.
3.  **Workers**: Mỗi Worker chỉ việc lấy tin từ Queue của mình và làm việc riêng (Gửi mail, tính tiền, trừ kho) mà không ảnh hưởng lẫn nhau.

### Khi nào dùng?
*   Hệ thống E-commerce, Microservices.
*   Khi muốn thêm tính năng mới (VD: Gửi SMS) mà không muốn sửa code của luồng cũ. Chỉ cần tạo thêm Queue mới và subscribe vào SNS.

---

## 5. Microservices with ECS Fargate (Container hóa không server)

Chạy Microservices bằng Docker mà không cần quản lý EC2 (OS patching, scaling).

### Mô hình
```mermaid
flowchart TD
    %% Styles
    classDef client fill:#4FC3F7,stroke:#0277BD,stroke-width:2px,color:white;
    classDef lb fill:#FFCA28,stroke:#FFA000,stroke-width:2px,color:black;
    classDef compute fill:#FF9800,stroke:#E65100,stroke-width:2px,color:white;
    classDef db fill:#BA68C8,stroke:#6A1B9A,stroke-width:2px,color:white,shape:cylinder;
    classDef net fill:#455A64,stroke:#37474F,stroke-width:2px,stroke-dasharray: 5 5,color:white;

    subgraph VPC ["VPC Cloud"]
        direction TB
        
        subgraph PublicSubnet ["Public Subnet"]
            ALB["Application Load Balancer"]:::lb
        end

        subgraph PrivateApp ["Private Subnet (Fargate)"]
             Svc1["Container: User Service"]:::compute
             Svc2["Container: Order Service"]:::compute
             Svc3["Container: Payment Service"]:::compute
        end
        
        subgraph PrivateData ["Private Subnet (Data)"]
             Aurora[("Aurora Serverless")]:::db
        end
    end

    User((User)):::client --> ALB
    ALB --> Svc1 & Svc2 & Svc3
    Svc1 & Svc2 & Svc3 --> Aurora
```

### Chi tiết
1.  **Fargate**: AWS tự tìm server trống để chạy container của bạn. Bạn chỉ cần đưa Docker Image.
2.  **Aurora Serverless**: Database tự scale theo tải, tắt khi không dùng. Kết hợp với Fargate tạo thành bộ đôi "Serverless hoàn hảo" cho Microservice.

### Khi nào dùng?
*   Chuyển đổi Monolith sang Microservices.
*   Không muốn tốn nhân sự quản lý hạ tầng (SysAdmin/DevOps).

---

## 6. Global Application (Multi-Region Disaster Recovery)

Kiến trúc "Bất tử". Chạy trên nhiều Region khác nhau. Region A sập, Region B gánh.

### Mô hình
```mermaid
flowchart TB
    %% Styles
    classDef client fill:#4FC3F7,stroke:#0277BD,stroke-width:2px,color:white;
    classDef dns fill:#FFCA28,stroke:#FFA000,stroke-width:2px,color:black;
    classDef region fill:#455A64,stroke:#90A4AE,stroke-width:2px,stroke-dasharray: 5 5,color:white;
    classDef compute fill:#FF9800,stroke:#E65100,stroke-width:2px,color:white;
    classDef db fill:#BA68C8,stroke:#6A1B9A,stroke-width:2px,color:white,shape:cylinder;

    User((User)):::client --> R53["Route 53\n(Latency / Failover Policy)"]:::dns

    subgraph RegionA ["Region: US-EAST-1 (Primary)"]
        AppA["EC2 / Lambda"]:::compute
        DB_A[("DynamoDB\nGlobal Table")]:::db
    end

    subgraph RegionB ["Region: AP-SOUTHEAST-1 (Backup)"]
        AppB["EC2 / Lambda"]:::compute
        DB_B[("DynamoDB\nGlobal Table")]:::db
    end

    R53 -- "Gần Mỹ hơn" --> AppA
    R53 -- "Gần VN hơn" --> AppB
    
    AppA <--> DB_A
    AppB <--> DB_B
    DB_A <--> DB_B:::db
    
    linkStyle 4,5 stroke:#4CAF50,stroke-width:2px;
```

### Chi tiết
1.  **Route 53**: Điều hướng người dùng. Nếu người dùng ở Việt Nam -> Vào Region Singapore (Region B). Nếu Singapore sập -> Tự động trỏ về Mỹ (Region A).
2.  **DynamoDB Global Table**: Dữ liệu ghi vào Mỹ sẽ tự động sync sang Singapore ngay lập tức (Multi-Master). Ghi ở đâu cũng được.

### Khi nào dùng?
*   Ứng dụng toàn cầu (Uber, Netflix).
*   Yêu cầu thời gian uptime 99.999%.

---

## 7. Hybrid Cloud Storage (On-premise mở rộng lên Cloud)

Doanh nghiệp vẫn dùng server vật lý ở công ty nhưng muốn backup dữ liệu lên Cloud.

### Mô hình
```mermaid
flowchart LR
    %% Styles
    classDef onprem fill:#607D8B,stroke:#263238,stroke-width:2px,color:white;
    classDef aws fill:#FF9800,stroke:#E65100,stroke-width:2px,color:white;
    classDef gateway fill:#9C27B0,stroke:#4A148C,stroke-width:2px,color:white;
    classDef storage fill:#8BC34A,stroke:#33691E,stroke-width:2px,color:white,shape:cylinder;

    subgraph Company ["Công ty (On-Premise Data Center)"]
        Server["Server Vật lý"]:::onprem
        SG["Storage Gateway\n(File Gateway)"]:::gateway
    end

    subgraph AWS ["AWS Cloud"]
        S3[("S3 Bucket\n(Backup)")]:::storage
    end

    Server -- "NFS / SMB" --> SG
    SG -- "HTTPS / Direct Connect" --> S3
```

### Chi tiết
1.  **Storage Gateway**: Là một máy ảo cài đặt tại Server của công ty. Nó giả lập một ổ cứng mạng.
2.  **Cơ chế**: Server công ty copy file vào ổ cứng ảo này -> Storage Gateway âm thầm upload file đó lên S3.

### Khi nào dùng?
*   Mở rộng dung lượng lưu trữ cho Data Center cũ mà không cần mua thêm ổ cứng vật lý.
*   Backup dữ liệu lên Cloud để an toàn (Disaster Recovery).


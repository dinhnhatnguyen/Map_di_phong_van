# Storage

> Gặp từ viết tắt không quen? Xem **[Từ Điển](../00-start-here/glossary.md)**.

SAA-C03 hay hỏi storage theo kiểu dữ liệu, access pattern, performance, durability, retention, encryption và cost. Hãy bắt đầu bằng câu hỏi: object, block hay file?

## Storage Map

| Service | Loại | Dùng khi | Bẫy đề |
|---|---|---|---|
| S3 | Object | file, backup, static site, data lake, log, archive | Không mount như file system POSIX cho app truyền thống |
| S3 Glacier | Archive object | long-term archive, compliance retention | Retrieval có delay/cost tùy class |
| EBS | Block | volume gắn EC2 trong một AZ | Không share ngang nhiều EC2 phổ thông như EFS |
| EFS | File NFS | shared Linux file system, nhiều EC2/Lambda/ECS | Đắt hơn S3, không dùng cho static object scale lớn |
| FSx for Windows | File SMB | Windows file share, AD integration | Không chọn EFS cho Windows SMB |
| FSx for Lustre | File HPC | HPC, ML, high throughput, S3-linked data | Không phải general backup service |
| AWS Backup | Managed backup | central backup policy đa service | Không thay thế replication strategy realtime |
| Storage Gateway | Hybrid storage | on-prem app cần file/tape/volume gateway lên AWS | Cần appliance/VM ở on-prem |

Nguồn: [Amazon S3 storage classes](https://aws.amazon.com/s3/storage-classes/), [S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/), [EBS User Guide](https://docs.aws.amazon.com/ebs/), [EFS User Guide](https://docs.aws.amazon.com/efs/).

## S3 High-Yield

### Khi Nào Chọn S3

- Static website, image/video, backup, logs, data lake.
- Cần durability rất cao.
- Cần lifecycle sang storage class rẻ hơn.
- Cần event trigger Lambda/SQS/SNS/EventBridge khi object thay đổi.
- Cần cross-account/object-level access bằng bucket policy/IAM.

### Storage Classes

| Class | Access pattern | Min duration | Ghi nhớ thi |
|---|---|---:|---|
| S3 Standard | Frequent access | — | Default, low latency, multi-AZ |
| S3 Intelligent-Tiering | Unknown/changing access | — | Tự chuyển tier; object < 128KB không tự chuyển tier nên không tiết kiệm được |
| S3 Standard-IA | Infrequent access, cần lấy nhanh | 30 ngày | Rẻ hơn storage, có retrieval fee |
| S3 One Zone-IA | Infrequent, có thể mất nếu AZ mất | 30 ngày | Rẻ hơn, không dùng cho critical data duy nhất |
| S3 Glacier Instant Retrieval | Archive nhưng cần milliseconds access | 90 ngày | Archive frequent enough to need instant |
| S3 Glacier Flexible Retrieval | Archive hiếm truy cập | 90 ngày | Retrieval từ phút đến giờ (Expedited/Standard/Bulk) |
| S3 Glacier Deep Archive | Archive dài hạn rất rẻ | 180 ngày | Retrieval 12-48 giờ; dùng compliance/long retention |
| S3 Express One Zone | Very high performance single-AZ | — | Dữ liệu latency-sensitive trong một AZ, single-AZ |

### Lifecycle

- Transition: chuyển object sang class rẻ hơn sau N ngày.
- Expiration: xóa object sau N ngày.
- Noncurrent version lifecycle: xử lý version cũ.
- Dùng lifecycle khi access pattern theo tuổi dữ liệu rõ ràng.
- Dùng Intelligent-Tiering khi không đoán được access pattern.
- Bẫy đề: lifecycle transition có minimum storage duration; chuyển object sang IA/Glacier trước hạn vẫn tính đủ phí tối thiểu.

### Security

- Block Public Access: bật mặc định cho bucket không cần public.
- Bucket policy: resource-based policy.
- IAM policy: identity-based policy.
- ACL: legacy, hạn chế dùng.
- Object Ownership bucket owner enforced: tắt ACL, bucket owner sở hữu object.
- Encryption:
  - SSE-S3: AWS quản lý key.
  - SSE-KMS: dùng KMS key, audit/control tốt hơn.
  - DSSE-KMS: dual-layer encryption.
  - SSE-C: customer-provided key, ít chọn trong SAA.
  - Client-side encryption: mã hóa trước khi upload.
- S3 Object Lock:
  - Governance mode: user có quyền đặc biệt có thể bypass.
  - Compliance mode: không ai xóa/sửa trước retention date, kể cả root.

### Replication

- Same-Region Replication: compliance/log aggregation.
- Cross-Region Replication: DR, low latency read ở Region khác.
- Cần bật versioning trên source và destination.
- Replication không retroactive cho object cũ trừ khi dùng S3 Batch Replication.

### Performance

- Multipart upload cho object lớn.
- Transfer Acceleration dùng edge network để upload/download xa Region.
- CloudFront cho caching/download global.
- S3 Select dùng query một phần dữ liệu trong object để giảm transfer.

## EBS High-Yield

### Ghi Nhớ

- EBS là block storage trong một AZ, gắn với EC2.
- Snapshot lưu trên S3-managed backend và có thể copy cross-Region.
- Volume có thể resize/type change online trong nhiều trường hợp.
- Encryption dùng KMS.
- Root EBS thường bị xóa khi terminate nếu DeleteOnTermination bật.

### Volume Types

| Type | Dùng khi |
|---|---|
| gp3 | General purpose, cấu hình IOPS/throughput độc lập, default tốt |
| gp2 | Legacy general purpose, burst theo size |
| io2 / io2 Block Express | Mission-critical DB, IOPS cao, latency thấp |
| st1 | Throughput optimized HDD, big data/log sequential |
| sc1 | Cold HDD, ít truy cập, cost thấp |

Bẫy đề:

- EBS volume không tự multi-AZ. Muốn HA app thì dùng ASG multi-AZ và dữ liệu trên managed DB/S3/EFS.
- Muốn share file giữa nhiều EC2 Linux: EFS, không phải EBS.
- Muốn high IOPS DB trên EC2: io2 hoặc gp3 tùy requirement.

## EFS High-Yield

- Managed NFS file system cho Linux.
- Mount từ nhiều EC2/ECS/Lambda qua nhiều AZ.
- Regional EFS lưu nhiều AZ; One Zone EFS rẻ hơn nhưng ít HA hơn.
- Performance modes: General Purpose và Max I/O.
- Throughput modes: Bursting, Provisioned, Elastic.
- Lifecycle sang EFS Infrequent Access hoặc Archive để giảm cost.
- Access Points giúp enforce path/user/permission cho app.

Chọn EFS khi đề nói:

- "multiple Linux EC2 instances need shared file access"
- "POSIX-compliant shared storage"
- "container tasks need common filesystem"

## FSx High-Yield

| Service | Dùng khi |
|---|---|
| FSx for Windows File Server | SMB, Windows app, Active Directory |
| FSx for Lustre | HPC/ML, high throughput, link với S3 |
| FSx for NetApp ONTAP | Enterprise NFS/SMB/iSCSI, snapshots, replication |
| FSx for OpenZFS | OpenZFS managed file system, low latency |

## AWS Backup

- Central backup policy cho EBS, RDS, DynamoDB, EFS, FSx, Storage Gateway và nhiều service khác.
- Backup vault có access policy và Vault Lock.
- Dùng khi đề yêu cầu centralized backup/compliance across accounts/services.

## Exam Decision Rules

| Requirement | Chọn |
|---|---|
| Static files/data lake/logs | S3 |
| Archive compliance 7-10 năm, cực rẻ | S3 Glacier Deep Archive |
| Unknown access pattern | S3 Intelligent-Tiering |
| Shared Linux file system | EFS |
| Windows SMB share | FSx for Windows |
| HPC high throughput file system | FSx for Lustre |
| EC2 boot/data disk | EBS |
| Centralized backups | AWS Backup |
| On-prem NFS/SMB cache to S3 | Storage Gateway File Gateway |
| Immutable retention | S3 Object Lock hoặc AWS Backup Vault Lock |

## Liên Kết

- [Compute](compute.md)
- [Networking](networking.md)
- [Migration Analytics Edge](migration-analytics-edge.md)
- [Service Selection Matrix](../02-architecture/service-selection-matrix.md)

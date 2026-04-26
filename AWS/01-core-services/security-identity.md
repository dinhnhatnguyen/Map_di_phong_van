# Security And Identity

Security là domain lớn nhất của SAA-C03. Hãy nhớ: đề thường ưu tiên least privilege, managed identity, encryption, private access, auditability và centralized governance.

## Identity Map

| Service/Feature | Dùng khi | Bẫy đề |
|---|---|---|
| IAM User | Người/app cần long-term credential trong account | Tránh dùng access key lâu dài nếu có role/federation |
| IAM Group | Gán quyền cho nhiều user | Group không lồng group |
| IAM Role | Temporary credential cho AWS service/cross-account/federation | EC2/Lambda/ECS nên dùng role, không hardcode key |
| IAM Policy | JSON permission | Explicit deny thắng allow |
| STS | AssumeRole temporary credentials | Cross-account access thường dùng STS |
| IAM Identity Center | Workforce SSO/federation đa account | Thay cho tạo user thủ công trong từng account |
| Organizations | Quản lý nhiều account | Consolidated billing, OU, SCP |
| SCP | Guardrail permission max ở account/OU | SCP không tự cấp quyền, chỉ giới hạn |
| Control Tower | Landing zone multi-account | Dùng khi cần setup/governance chuẩn hóa |

Nguồn: [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/), [AWS Organizations](https://docs.aws.amazon.com/organizations/), [AWS KMS](https://docs.aws.amazon.com/kms/), [AWS Security Best Practices](https://docs.aws.amazon.com/security/).

## IAM Evaluation Logic

Quy tắc nhớ nhanh:

1. Mặc định là deny.
2. Explicit deny thắng mọi allow.
3. Cần allow từ identity/resource policy phù hợp.
4. Permission boundary, SCP, session policy chỉ giới hạn maximum permission.
5. SCP không ảnh hưởng service-linked roles (service-linked roles luôn có quyền mà service cần dù SCP giới hạn).

## Least Privilege Patterns

- EC2 truy cập S3: attach IAM role vào instance profile.
- Lambda truy cập DynamoDB: execution role chỉ allow table cần thiết.
- Cross-account access: account B tạo role trust account A; principal account A assume role.
- Human access nhiều account: IAM Identity Center + permission sets.
- Third-party SaaS access: external ID trong trust policy để tránh confused deputy.

## Resource Policies

| Resource policy | Use case |
|---|---|
| S3 bucket policy | Public/private bucket access, cross-account, deny non-TLS |
| KMS key policy | Ai được dùng/admin key |
| SQS queue policy | Cho SNS topic hoặc account khác gửi message |
| SNS topic policy | Cho account/service publish |
| Lambda resource policy | Cho API Gateway/EventBridge/account khác invoke |
| VPC endpoint policy | Giới hạn API qua endpoint |

## Encryption

| Requirement | Chọn |
|---|---|
| Mã hóa at rest managed đơn giản | SSE-S3, EBS/RDS default encryption |
| Cần control/audit key | KMS customer managed key |
| Cần dedicated HSM | CloudHSM |
| TLS cert public cho ALB/CloudFront/API Gateway | ACM |
| Rotate DB/API secrets | Secrets Manager |
| Config parameter/plain text hoặc secret cơ bản | Systems Manager Parameter Store |

### KMS High-Yield

- AWS managed key: AWS tạo/quản lý, ít control hơn.
- Customer managed key: bạn quản lý policy, rotation, grants, alias.
- Key policy là authority chính của KMS key.
- Envelope encryption: data key mã hóa dữ liệu, KMS key mã hóa data key.
- Multi-Region keys dùng cho use case client-side/global encryption nhưng không tự replicate data.
- KMS đạt FIPS 140-2 Level 2. CloudHSM đạt FIPS 140-2 Level 3 (dedicated hardware HSM).

Bẫy đề:

- Muốn decrypt object SSE-KMS cross-account: cần quyền S3 và quyền KMS key.
- Muốn force S3 encrypted upload: bucket policy deny nếu thiếu header encryption.
- CloudFront viewer HTTPS dùng ACM cert ở us-east-1.
- Khi đề nói "dedicated HSM" hoặc "FIPS 140-2 Level 3": chọn CloudHSM, không phải KMS.

## Application Security

| Service | Dùng khi |
|---|---|
| Cognito | User sign-up/sign-in, social/federated identity cho app |
| WAF | Chặn SQL injection, XSS, rate-based rules ở CloudFront/ALB/API Gateway |
| Shield Standard | DDoS protection mặc định |
| Shield Advanced | DDoS protection nâng cao, cost protection, DRT |
| Secrets Manager | Rotate secrets, DB credentials |
| ACM | TLS certificate management |

## Detection And Compliance

| Service | Dùng khi |
|---|---|
| CloudTrail | Audit API calls: ai làm gì, khi nào |
| CloudWatch Logs/Alarms | Logs, metrics, alarms |
| AWS Config | Resource configuration history/compliance rules |
| GuardDuty | Threat detection từ logs/signals |
| Inspector | Vulnerability scanning cho EC2/container/Lambda |
| Macie | Discover/classify sensitive data in S3 |
| Security Hub | Aggregate security findings |
| Detective | Investigate security findings |
| Audit Manager | Evidence collection cho audit |
| Artifact | Compliance reports/agreements |

## Network Security

- Public subnet chỉ để ALB/NAT/bastion nếu thật sự cần.
- Private subnet cho app/database.
- DB SG chỉ allow từ app SG.
- VPC endpoints giảm traffic qua internet.
- Network Firewall cho stateful managed network firewall.
- Firewall Manager quản lý WAF/Shield/SG policies nhiều account.

## Data Protection Checklist

- S3 Block Public Access bật.
- S3 versioning + MFA Delete/Object Lock nếu compliance.
- RDS/Aurora automated backup + Multi-AZ cho production.
- DynamoDB PITR cho recovery.
- EBS/RDS/S3/EFS encryption at rest.
- TLS in transit bằng ACM.
- KMS key policy và IAM policy đúng.
- Backup cross-Region/account nếu DR/compliance yêu cầu.

## Exam Decision Rules

| Requirement | Chọn |
|---|---|
| Multi-account governance | Organizations + SCP + Control Tower |
| Workforce SSO many accounts | IAM Identity Center |
| Temporary cross-account access | IAM Role + STS AssumeRole |
| App user authentication | Cognito |
| Store and rotate DB password | Secrets Manager |
| Encrypt with customer control | KMS customer managed key |
| Dedicated HSM | CloudHSM |
| TLS cert | ACM |
| SQLi/XSS/rate limit | WAF |
| DDoS advanced | Shield Advanced |
| Sensitive data discovery in S3 | Macie |
| Threat detection | GuardDuty |
| Vulnerability scanning | Inspector |
| Central security findings | Security Hub |

## Liên Kết

- [Networking](networking.md)
- [Storage](storage.md)
- [Integration Observability Cost](integration-observability-cost.md)

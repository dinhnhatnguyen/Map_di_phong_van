# Databases

> Gặp từ viết tắt không quen? Xem **[Từ Điển](../00-start-here/glossary.md)**.

SAA-C03 hỏi database theo access pattern: relational hay NoSQL, read-heavy hay write-heavy, global hay single Region, cần transaction hay key-value, cần managed hay tự quản.

## Database Map

| Service | Dùng khi | Tránh khi | Bẫy đề |
|---|---|---|---|
| RDS | SQL managed, MySQL/PostgreSQL/MariaDB/Oracle/SQL Server | Cần serverless NoSQL scale cực lớn | Multi-AZ là HA, Read Replica là scale đọc |
| Aurora | SQL compatible, performance/HA cao | Cần engine không hỗ trợ Aurora | Aurora storage replicate 6 copies across 3 AZ |
| DynamoDB | Serverless NoSQL key-value/document, massive scale | Query relational join phức tạp | Thiết kế partition key quyết định performance |
| ElastiCache | Cache/session/ranking, microsecond latency | Persistent source of truth duy nhất | Redis/Memcached khác nhau |
| Redshift | Data warehouse analytics OLAP | OLTP app transactions | Không thay RDS cho app DB |
| DocumentDB | MongoDB-compatible managed document DB | Cần native MongoDB full ecosystem | Dùng khi app MongoDB-compatible |
| Neptune | Graph DB | Relational/simple key-value | Social graph, recommendation, fraud graph |
| Keyspaces | Cassandra-compatible | Không cần Cassandra API | Serverless Cassandra use case |
| QLDB | Immutable ledger | Blockchain decentralized | Centralized ledger with cryptographic verification |

Nguồn: [Amazon RDS](https://docs.aws.amazon.com/rds/), [Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/), [Amazon DynamoDB](https://docs.aws.amazon.com/dynamodb/), [Amazon ElastiCache](https://docs.aws.amazon.com/elasticache/).

## RDS High-Yield

### Features

- Automated backup và point-in-time recovery.
- Manual snapshot giữ đến khi xóa.
- Multi-AZ deployment cho HA/failover.
- Read Replica cho read scaling và cross-Region read/DR pattern.
- Storage autoscaling.
- Performance Insights.
- RDS Proxy cho connection pooling với Lambda/app serverless.

### Multi-AZ vs Read Replica

| Tiêu chí | Multi-AZ | Read Replica |
|---|---|---|
| Mục tiêu chính | High availability/failover | Read scaling |
| Replication | Synchronous trong deployment cổ điển | Asynchronous |
| App đọc từ replica? | Thường không với standby cổ điển | Có |
| Failover automatic | Có | Không phải mục tiêu chính, có thể promote |
| Cross-Region | Multi-AZ là trong Region | Có thể cross-Region |

Ghi chú: RDS Multi-AZ DB cluster mới có readable standbys cho một số engine, nhưng trong đề Associate, rule nhanh vẫn là Multi-AZ = HA, Read Replica = scale read.

## Aurora High-Yield

- Tương thích MySQL/PostgreSQL.
- Storage tự scale và replicate 6 copies across 3 AZ.
- Cluster endpoint cho writer, reader endpoint cho read replicas.
- Aurora Replica scale đọc và failover nhanh.
- Aurora Serverless phù hợp workload variable/unpredictable, cần auto scale DB capacity.
- Global Database cho low-latency global reads và DR cross-Region.
- Backtrack cho Aurora MySQL có thể tua lại DB không cần restore snapshot trong một số tình huống.

Chọn Aurora khi đề nói:

- SQL compatible nhưng cần performance/availability cao hơn RDS thường.
- Read-heavy cần nhiều reader.
- Global read latency thấp với Aurora Global Database.
- Variable workload và muốn auto scale capacity.

## DynamoDB High-Yield

### Core Concepts

- Table, item, attribute.
- Primary key:
  - Partition key only.
  - Partition key + sort key.
- GSI: index với partition/sort key khác, eventual consistency.
- LSI: cùng partition key, sort key khác, tạo lúc tạo table.
- Capacity:
  - On-demand: traffic unpredictable, ít quản lý.
  - Provisioned: traffic predictable, có autoscaling/reserved capacity.
- Consistency:
  - Eventually consistent default.
  - Strongly consistent read trong cùng Region table/LSI.
- TTL: tự xóa item hết hạn.
- Streams: change data capture.
- DAX: in-memory cache cho read-heavy DynamoDB.
- Global Tables: multi-Region active-active.

### Bẫy Đề

- DynamoDB không giỏi ad-hoc query mọi field nếu không thiết kế index.
- DAX giảm read latency, không giúp write-heavy.
- Hot partition xảy ra khi partition key phân bố không đều.
- DynamoDB item size tối đa **400KB**; dùng S3 lưu object lớn và chỉ lưu pointer trong DynamoDB.
- LSI chỉ tạo được khi tạo table, không thể thêm sau; GSI có thể thêm bất cứ lúc nào.

## ElastiCache

| Engine | Dùng khi |
|---|---|
| Redis / Valkey | advanced data structures, persistence, pub/sub, ranking, session, replication |
| Memcached | cache đơn giản, scale ngang dễ, không cần persistence |

Use cases:

- Cache query DB read-heavy.
- Session store stateless web.
- Leaderboard/ranking với sorted sets.
- Rate limiting/token store.

Bẫy đề:

- Cache aside/lazy loading: app đọc cache trước, miss thì đọc DB rồi ghi cache.
- Write-through: ghi cache đồng thời với DB.
- Redis Multi-AZ + automatic failover cho HA.

## Redshift Và Analytics DB

- Redshift là data warehouse columnar cho OLAP.
- Redshift Spectrum query data ở S3.
- Không dùng Redshift cho transactional OLTP app.
- Athena query S3 bằng SQL, serverless, phù hợp ad-hoc query data lake.

## Database Migration

- Homogeneous migration: same engine, có thể dùng native tools hoặc DMS.
- Heterogeneous migration: khác engine, dùng AWS Schema Conversion Tool + DMS.
- DMS hỗ trợ ongoing replication/CDC để giảm downtime.

## Exam Decision Rules

| Requirement | Chọn |
|---|---|
| SQL app managed | RDS |
| SQL compatible performance/HA cao | Aurora |
| Read scaling SQL | Read Replica hoặc Aurora reader |
| HA SQL | Multi-AZ |
| Lambda nhiều connection tới RDS | RDS Proxy |
| Serverless NoSQL massive scale | DynamoDB |
| Global active-active NoSQL | DynamoDB Global Tables |
| DynamoDB read latency microseconds | DAX |
| Cache session/query | ElastiCache Redis/Valkey |
| Data warehouse OLAP | Redshift |
| Query S3 ad-hoc | Athena |
| MongoDB-compatible managed | DocumentDB |
| Graph relationships | Neptune |
| Cassandra-compatible | Keyspaces |
| Immutable centralized ledger | QLDB |

## Liên Kết

- [Storage](storage.md)
- [Integration Observability Cost](integration-observability-cost.md)
- [High-Yield Comparisons](../02-architecture/high-yield-comparisons.md)

---
title: "Worklog Tuần 5"
date: 2026-06-29
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

- **Ngày bắt đầu:** 29/06/2026
- **Ngày hoàn thành:** 04/07/2026

### Mục tiêu tuần 5:
- Nghiên cứu lý thuyết CSDL quan hệ Amazon RDS PostgreSQL (Multi-AZ, Read Replicas) & Caching Amazon ElastiCache Redis
- Tìm hiểu cơ chế quản lý phiên bản sơ đồ CSDL tự động bằng công cụ Flyway Database Migration
- Khởi chạy dự án Shopsflow E-Commerce: Thiết lập RDS PostgreSQL trong Private Subnets, cấu hình Redis Caching và chạy Flyway SQL Migration scripts

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc |
| --- | --- |
| 2 | Tìm hiểu lý thuyết Amazon RDS PostgreSQL: Cấu hình Multi-AZ cho tính sẵn sàng cao, DB Subnet Groups, Parameter Groups, Automated Backups và Read Replicas. |
| 3 | Nghiên cứu giải pháp đệm dữ liệu Amazon ElastiCache cho Redis: Cấu trúc In-Memory Caching, Redis Clusters, eviction policies và cơ chế quản lý phiên bản CSDL Flyway SQL Migration. |
| 4 | Thực hành bài Lab AWS (RDS PostgreSQL & Redis Setup): Tạo DB Subnet Group, khởi tạo RDS PostgreSQL 16 trong Private DB Subnets và tạo ElastiCache Redis Cluster. |
| 5 | Tích hợp Spring Data JPA & Redis Caching vào backend Spring Boot dự án Shopsflow; soạn các tệp Flyway migration SQL (`V1__init_schema.sql`, `V2__seed_demo_data.sql`). |
| 6 | Thực thi Flyway migration tự động tạo bảng (Users, Categories, Products, Orders, Reviews), kiểm thử kết nối CSDL và tổng hợp báo cáo thu hoạch tuần 5. |

### Kết quả đạt được tuần 5:
* Hoàn thành đúng tiến độ công việc tuần 5 (RDS, Redis Labs & Shopsflow Database Setup).
* Khởi tạo thành công cơ sở dữ liệu quan hệ RDS PostgreSQL và đệm Redis cho ứng dụng Shopsflow.

### Nguồn tài liệu hướng dẫn tham khảo:
- [Amazon RDS PostgreSQL User Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html)
- [Amazon ElastiCache for Redis Guide](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/)
- [Flyway Database Migration Tool Documentation](https://documentation.red-gate.com/flyway)

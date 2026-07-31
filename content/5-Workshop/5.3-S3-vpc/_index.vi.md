---
title: "Triển khai Database & Backend API"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Phần này hướng dẫn khởi tạo **Amazon RDS PostgreSQL** làm cơ sở dữ liệu chính và triển khai **Spring Boot Backend API** lên **EC2 instances** bên trong Private Subnets, đặt sau **Application Load Balancer (ALB)**.

### Tổng quan kiến trúc

```
Internet
    │
    ▼
[ALB - Public Subnets]  (shopsflow-alb-sg: cho phép :80 từ 0.0.0.0/0)
    │
    ▼
[EC2 ASG - Private App Subnets]  (shopsflow-ec2-sg: cho phép :8080 từ ALB)
    │  └── Spring Boot API (Docker) ← Secrets Manager (DB creds, JWT, VNPay)
    ▼
[RDS PostgreSQL - Private DB Subnets]  (shopsflow-rds-sg: cho phép :5432 từ EC2)
    Multi-AZ: Primary (AZ-a) + Standby (AZ-b)
```

#### Nội dung

1. [Khởi tạo RDS PostgreSQL](5.3.1-rds-database/)
2. [Triển khai Spring Boot Backend API](5.3.2-ec2-backend/)
---
title: "Deploy Database & Backend API"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

This section covers provisioning the **Amazon RDS PostgreSQL** database and deploying the **Spring Boot Backend API** onto **EC2 instances** inside the Private Subnets, positioned behind the **Application Load Balancer (ALB)**.

### Architecture Overview

```
Internet
    │
    ▼
[ALB - Public Subnets]  (shopsflow-alb-sg: allows :80 from 0.0.0.0/0)
    │
    ▼
[EC2 ASG - Private App Subnets]  (shopsflow-ec2-sg: allows :8080 from ALB only)
    │  └── Spring Boot API (Docker) ← Secrets Manager (DB creds, JWT, VNPay)
    ▼
[RDS PostgreSQL - Private DB Subnets]  (shopsflow-rds-sg: allows :5432 from EC2 only)
    Multi-AZ: Primary (AZ-a) + Standby (AZ-b)
```

#### Content

1. [Provision RDS PostgreSQL Database](5.3.1-rds-database/)
2. [Deploy Spring Boot Backend API](5.3.2-ec2-backend/)
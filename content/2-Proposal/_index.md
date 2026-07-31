---
title: "Proposal"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Shopsflow — Full-Stack E-Commerce Application on AWS
## Enterprise Cloud Solution: High Availability, Security & Infrastructure Automation

### 1. Executive Summary
Shopsflow is a modern, full-stack E-Commerce platform designed and deployed on AWS Cloud Infrastructure. The platform serves online customer shopping needs (product browsing, cart management, checkout, VNPay Sandbox payment gateway, Cloudinary image uploads, product reviews) and provides a dedicated administrative dashboard for Admins (category management, product inventory, order state transitions `PENDING → PAID → SHIPPED → DELIVERED`, and Admin Reviews Moderation).

The cloud infrastructure incorporates core AWS services including Amazon EC2, Amazon RDS PostgreSQL Multi-AZ, ElastiCache Redis, Amazon CloudWatch Observability, AWS Secrets Manager, and is 100% automated using Terraform Modules IaC.

---

### 2. Problem Statement

#### What’s the Problem?
Small and medium retail businesses operating traditional e-commerce web applications face significant operational hurdles:
- **Manual Infrastructure Management**: High vulnerability to network congestion and unexpected downtime during peak traffic.
- **Database Data Loss Risks**: Running databases on standalone VPS lacks automated backups and high availability mechanisms.
- **Security Secret Leaks**: Storing JWT secrets and DB credentials in plain text configuration files inside GitHub repositories.
- **Lack of Centralized Monitoring**: Difficulty detecting system outages or traffic surges without automated logging and alert systems.

#### The Solution
The **Shopsflow on AWS** project addresses these challenges through:
- **Modern Full-Stack Architecture**: React 19 (Vite, TypeScript, Nginx) on Frontend and Spring Boot (Java 21, JPA, Flyway Migration) on Backend.
- **Managed AWS Cloud Database**: Amazon RDS PostgreSQL Multi-AZ for 99.99% data durability and ElastiCache Redis for high-speed in-memory caching.
- **IaC Automation & Security**: Reusable Terraform Modules for cloud provisioning, AWS Secrets Manager for secret storage, and IAM Least Privilege access rules.
- **Real-Time Monitoring & Alerts**: CloudWatch Dashboards, Log Groups, and CloudWatch Alarms dispatching automated email / Slack incident alerts.

#### Benefits and Return on Investment (ROI)
- **High Availability & Reliability**: Eliminates single points of failure with Multi-AZ deployment and ALB load balancers.
- **DevOps Operational Automation**: Saves 80% of infrastructure setup time using reusable Terraform IaC code.
- **FinOps Cost Optimization**: Provisions resources strictly on-demand, monitored via AWS Billing Alarms.

---

### 3. Solution Architecture

Shopsflow follows a standard 3-Tier Enterprise Cloud Architecture deployed inside an AWS Custom VPC Multi-AZ topology:

![Shopsflow AWS Cloud Architecture](/images/2-Proposal/architecture.png)

#### AWS Services Used:
- **Amazon EC2**: Hosts web applications (Nginx Frontend & Spring Boot Backend) in Public/Private Subnets.
- **Amazon RDS for PostgreSQL**: Managed relational database in Private DB Subnets with Multi-AZ failover.
- **Amazon ElastiCache (Redis)**: In-memory product catalog caching for sub-millisecond query performance.
- **Application Load Balancer (ALB)**: Distributes incoming web traffic with HTTPS SSL termination.
- **AWS Secrets Manager**: Securely stores JWT Secret Keys and database connection credentials.
- **Amazon CloudWatch**: Centralized log collection, metrics dashboard visualization, and threshold alarms.
- **AWS IAM**: EC2 Instance Profile roles enforcing least-privilege permission policies.
- **AWS S3 & DynamoDB**: Remote Terraform State storage and DynamoDB State Locking.

---

### 4. Technical Implementation

#### Implementation Phases:
1. **Month 1 (Weeks 1 - 4)**: Learn AWS fundamentals, design Database schema, initialize Spring Boot application, and finalize Proposal.
2. **Month 2 (Weeks 5 - 9)**: Develop REST APIs (Cart, Order, Payment), handle Concurrency, write Unit Tests, complete Blog documentation, and package final project report.

#### Core Technical Requirements:
- **Backend**: Java 21, Spring Boot 3, Spring Security JWT, Spring Data JPA, Flyway Database Migration, JUnit 5 & Mockito Unit Testing.
- **Frontend**: React 19, TypeScript, Vite, Axios, Nginx 1.25 reverse proxy.
- **Database & Cache**: PostgreSQL 16, Redis 7.
- **Infrastructure as Code**: Terraform v1.5+, AWS Provider, HCL Modules.

---

### 5. Budget Estimation

- **AWS Free Tier Coverage**: Most services (EC2 t3.micro, S3 5GB, CloudWatch 5GB Logs) operate within Free Tier limits for lab demonstration.
- **Cost Controls**: CloudWatch Billing Alarms notify email immediately if expenses reach $5 USD, with resource cleanup executed post-evaluation (FinOps Best Practices).

---

### 6. Risk Assessment

| Risk | Impact | Mitigation Strategy |
| --- | --- | --- |
| **Secret key / DB password leaks** | High | Store all secrets in AWS Secrets Manager & IAM Roles; exclude `.env` files from Git commits. |
| **Unauthorized RDS database access** | High | Place RDS in Private DB Subnet, allowing inbound connections exclusively from EC2 Security Groups. |
| **Network congestion / CPU spikes** | Medium | Utilize ALB traffic distribution and configure CloudWatch Metric Alarms for proactive monitoring. |
| **Budget overruns on AWS** | Low | Set CloudWatch Billing Alarms and terminate temporary demo resources immediately post-defense. |

---

### 7. Expected Outcomes

1. **Production-Ready E-Commerce Web Application**: Online customers complete VNPay checkout smoothly; Admins manage orders and moderate product reviews effectively.
2. **Automated DevOps Cloud Infrastructure**: 100% cloud resources provisioned via modular, reusable Terraform HCL scripts.
3. **High-Quality Internship Capstone Report**: Comprehensive internship report complete with 9-week worklogs, AWS architecture diagrams, and system validation documentation.
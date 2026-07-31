---
title: "Introduction & Architecture"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

### 1. Project Vision & Objectives

#### Context & Problem Statement
The **Shopsflow** system is a full-stack e-commerce application featuring a Customer Storefront for browsing products, shopping cart management, and online payment integration via **VNPay Sandbox**, alongside an Admin Portal for catalog management, order tracking, and customer review moderation.

The architecture is tailored for retail businesses looking to migrate onto AWS Cloud infrastructure with optimal cost, high availability, and complete ownership over source code and database assets.

Key challenges addressed by this deployment:
* **Zero Downtime & Environment Consistency:** Containerized deployment (Docker) ensures environment parity between development and cloud runtime.
* **Strict Data Isolation:** Complete isolation of Backend API servers and PostgreSQL database inside Private Subnets, barring all direct public Internet access.
* **Data Resilience:** Automated scheduled PostgreSQL database backups transferred via AWS private network (VPC Gateway Endpoint) to prevent data loss.
* **Centralized Observability:** Centralized application logs and OS metrics shipped directly to CloudWatch for proactive incident management.

#### Core Deliverables (Outputs)
* **Frontend Web:** React + Vite Single Page Application (SPA) statically hosted on Amazon S3 and distributed globally via Amazon CloudFront CDN.
* **Backend API:** Spring Boot RESTful API executing on EC2 within Private Subnets, managed by an Auto Scaling Group (ASG) behind an Application Load Balancer (ALB).
* **Database RDS:** PostgreSQL Database running in Multi-AZ Standby mode with public access explicitly disabled (`Publicly Accessible: No`).
* **Security & Encryption:** Secrets and JWT credentials stored in AWS Secrets Manager encrypted with KMS Customer Managed Keys. Edge protection enabled via AWS WAF.
* **Monitoring & Backup:** Centralized CloudWatch Dashboards, Log Groups, and Alarms. Compressed database dumps uploaded to S3 via VPC Gateway Endpoint.

#### Program Alignment (First Cloud AI Journey - FCAJ)
The project utilizes core AWS services including: **VPC**, **EC2**, **RDS**, **CloudFront**, **WAF**, **S3**, **Secrets Manager**, **KMS**, **CloudWatch**, and **IAM**. The infrastructure aligns strictly with the AWS Well-Architected Framework principles of Security and High Availability.

---

### 2. Architecture Diagram & Technical Design

#### Architecture Diagram

The diagram below illustrates the multi-tier architecture and data flow of the Shopsflow system on AWS:

![Architecture Diagram](/images/5-Workshop/5.1-Workshop-overview/diagram.png)

#### Service Selection Rationale

* **Amazon CloudFront & Amazon S3 (Frontend):**
  * Statically hosting frontend code on S3 and distributing via CloudFront offloads computing requirements from EC2, reduces global latency via Edge Caching, and lowers cost. AWS WAF provides edge security.
* **AWS ALB & Auto Scaling Group (EC2 Backend):**
  * ALB routes traffic across EC2 instances in Private Subnets across two Availability Zones (AZs) for high availability. Auto Scaling Group dynamically handles CPU spikes.
* **Amazon RDS PostgreSQL Multi-AZ:**
  * Synchronously replicates data to a Standby instance in AZ-b. In case of primary failure, RDS automatically fails over to the Standby database without application disruption.
* **AWS Secrets Manager & KMS:**
  * Centrally stores sensitive values (Database credentials, JWT secret) encrypted with KMS. EC2 dynamically fetches secrets during startup via IAM role authorization.
* **VPC Gateway Endpoint (S3):**
  * Enables EC2 instances in Private Subnets to upload database backups directly to S3 via AWS internal network, bypassing NAT Gateway and eliminating data transfer fees.

#### Security Control Chain
Security Groups enforce strict chained network rules:
`Internet` → `CloudFront (WAF)` → `ALB SG (Ports 80/443)` → `EC2 SG (Port 8080 from ALB SG)` → `RDS SG (Port 5432 from EC2 SG)`.
---
title: "Shopsflow Workshop on AWS"
date: 2026-06-15
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Shopsflow — Full-Stack E-Commerce Application Deployment on AWS

#### Overview

This workshop provides step-by-step instructions for deploying the complete **Shopsflow** E-Commerce system onto **AWS Cloud** infrastructure following a secure, highly available 3-Tier Architecture.

The **Shopsflow** system includes:
- **Customer Storefront:** React + Vite SPA for product browsing, cart management, and checkout via the **VNPay Sandbox** payment gateway.
- **Admin Portal:** Product catalog management, order tracking, inventory control, and customer review moderation.
- **Spring Boot Backend API:** RESTful API handling all business logic including JWT authentication, role-based access control (Customer / Admin), and asynchronous order processing.
- **AWS Infrastructure:** All cloud resources managed via **Terraform Modules IaC**, secured with **Secrets Manager & KMS**, and monitored via **CloudWatch**.

After completing this workshop, you will be able to:
* Design and deploy a **Multi-AZ VPC** infrastructure with multi-tier security zoning.
* Configure **ALB + Auto Scaling Groups** to deliver high availability for the Backend API.
* Operate **RDS PostgreSQL Multi-AZ** with automated database backup via S3 Gateway Endpoint.
* Protect the application with **AWS WAF**, **Secrets Manager**, **KMS**, and **IAM Least Privilege**.
* Implement comprehensive monitoring with **CloudWatch Dashboards, Alarms, and Log Groups**.

---

#### Workshop Content

1. [Introduction & Architecture](5.1-Workshop-overview/)
2. [Network Setup, Access Control & Secrets](5.2-Prerequiste/)
3. [Deploy Database & Backend API (RDS + EC2 + ALB)](5.3-S3-vpc/)
4. [Deploy Frontend (S3 + CloudFront + WAF)](5.4-S3-onprem/)
5. [Monitoring & Backup (CloudWatch + S3)](5.5-Policy/)
6. [Clean Up Resources](5.6-Cleanup/)
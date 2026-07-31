---
title: "Workshop Shopsflow on AWS"
date: 2026-06-15
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Shopsflow — Triển khai Ứng dụng E-Commerce Full-Stack trên AWS

#### Tổng quan

Workshop này hướng dẫn từng bước triển khai hoàn chỉnh hệ thống E-Commerce **Shopsflow** lên hạ tầng **AWS Cloud** theo mô hình kiến trúc 3 lớp (3-Tier Architecture) đạt chuẩn bảo mật và sẵn sàng cao.

Hệ thống **Shopsflow** bao gồm:
- **Giao diện Khách hàng (Storefront):** React + Vite SPA để duyệt sản phẩm, quản lý giỏ hàng và thanh toán qua cổng **VNPay Sandbox**.
- **Giao diện Quản trị (Admin Portal):** Quản lý danh mục, sản phẩm, đơn hàng và kiểm duyệt đánh giá của khách hàng.
- **Backend API (Spring Boot):** RESTful API xử lý toàn bộ nghiệp vụ, bao gồm xác thực JWT, phân quyền theo vai trò (Customer / Admin) và xử lý đơn hàng bất đồng bộ.
- **Hạ tầng AWS:** Toàn bộ tài nguyên được quản lý bằng mã nguồn **Terraform Modules IaC**, bảo mật bằng **Secrets Manager & KMS**, giám sát bởi **CloudWatch**.

Sau khi hoàn thành workshop, bạn sẽ có khả năng:
* Thiết kế và triển khai hạ tầng **VPC Multi-AZ** phân vùng bảo mật nhiều tầng.
* Cấu hình **ALB + Auto Scaling Group** cung cấp tính sẵn sàng cao cho Backend API.
* Vận hành **RDS PostgreSQL Multi-AZ** và cơ chế backup dữ liệu tự động qua S3 Gateway Endpoint.
* Bảo vệ ứng dụng bằng **AWS WAF**, **Secrets Manager**, **KMS** và **IAM Least Privilege**.
* Giám sát toàn diện với **CloudWatch Dashboards, Alarms và Log Groups**.

---

#### Nội dung Workshop

1. [Giới thiệu & Kiến trúc](5.1-Workshop-overview/)
2. [Thiết lập Mạng, Quyền & Secrets](5.2-Prerequiste/)
3. [Triển khai Database & Backend API (RDS + EC2 + ALB)](5.3-S3-vpc/)
4. [Triển khai Frontend (S3 + CloudFront + WAF)](5.4-S3-onprem/)
5. [Giám sát & Backup (CloudWatch + S3)](5.5-Policy/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)
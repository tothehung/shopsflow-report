---
title: "Worklog Tuần 6"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

- **Ngày bắt đầu:** 06/07/2026
- **Ngày hoàn thành:** 11/07/2026

### Mục tiêu tuần 6:
- Nghiên cứu lý thuyết Infrastructure as Code (IaC) với HCL Terraform, State Management & Remote Lock với S3/DynamoDB
- Thực hành bài Lab AWS: Tự động hóa khởi tạo hạ tầng Cloud bằng bộ mã Terraform Modules tái sử dụng
- Hoàn thiện các module Terraform (VPC, EC2, RDS, ALB) cho ứng dụng Shopsflow và thực thi `terraform plan` / `apply`

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc |
| --- | --- |
| 2 | Nghiên cứu lý thuyết Infrastructure as Code (IaC) với HashiCorp Terraform: Cú pháp HCL, Terraform State, State Locking với DynamoDB và Remote Backend trên S3. |
| 3 | Thiết kế kiến trúc Terraform Modules tái sử dụng: Phân tách các module `vpc`, `ec2`, `rds_postgres`, `alb` và định nghĩa các tệp `variables.tf`, `outputs.tf`, `main.tf`. |
| 4 | Thực hành bài Lab AWS (Infrastructure Automation with Terraform): Viết mã Terraform khởi tạo Custom VPC Multi-AZ, Subnets, Internet Gateway và NAT Gateway. |
| 5 | Hoàn thiện mã nguồn Terraform Module khởi tạo máy chủ EC2 Linux, RDS PostgreSQL và Application Load Balancer (ALB) cho dự án Shopsflow. |
| 6 | Tiến hành kiểm thử lệnh `terraform fmt`, `terraform validate`, chạy `terraform plan` & `apply` khởi tạo hạ tầng tự động và tổng hợp báo cáo tuần 6. |

### Kết quả đạt được tuần 6:
* Hoàn thành đúng tiến độ công việc tuần 6 (Terraform IaC Labs & Shopsflow Infrastructure Automation).
* Tự động hóa 100% quy trình tạo môi trường Cloud cho ứng dụng Shopsflow bằng mã nguồn Terraform.

### Nguồn tài liệu hướng dẫn tham khảo:
- [HashiCorp Terraform AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Terraform Backend S3 & DynamoDB State Locking Guide](https://developer.hashicorp.com/terraform/language/settings/backends/s3)
- [Terraform Modules Best Practices](https://developer.hashicorp.com/terraform/language/modules)

---
title: "Worklog Week 6"
date: 2026-07-06
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

- **Start Date:** July 06, 2026
- **Completion Date:** July 11, 2026

### Objectives for Week 6:
- Theoretical research on Infrastructure as Code (IaC) with HCL Terraform, State Management & Remote State Locking via S3/DynamoDB
- Hands-on AWS Lab: Cloud Infrastructure Automation using reusable Terraform Modules
- Complete Terraform Modules (VPC, EC2, RDS, ALB) for Shopsflow application and execute `terraform plan` / `apply`

### Tasks to implement this week:
| Day | Task |
| --- | --- |
| 2 | Study Infrastructure as Code (IaC) theory with HashiCorp Terraform: HCL syntax, Terraform State files, DynamoDB State Locking, and S3 Remote Backend architecture. |
| 3 | Design reusable Terraform Modules architecture: Modularize `vpc`, `ec2`, `rds_postgres`, `alb` and define `variables.tf`, `outputs.tf`, `main.tf` structures. |
| 4 | Hands-on AWS Lab (Infrastructure Automation with Terraform): Write Terraform HCL code provisioning Multi-AZ Custom VPC, Subnets, Internet Gateway, and NAT Gateway. |
| 5 | Complete Terraform Modules for Shopsflow EC2 Linux instances, RDS PostgreSQL database, and Application Load Balancer (ALB). |
| 6 | Execute `terraform fmt`, `terraform validate`, run `terraform plan` & `apply` for automated cloud environment setup, and compile Week 6 report. |

### Results achieved in Week 6:
* Completed Week 6 tasks on schedule (Terraform IaC Labs & Shopsflow Infrastructure Automation).
* Automated 100% of cloud environment provisioning for Shopsflow using Terraform scripts.

### References & Study Materials:
- [HashiCorp Terraform AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Terraform Backend S3 & DynamoDB State Locking Guide](https://developer.hashicorp.com/terraform/language/settings/backends/s3)
- [Terraform Modules Best Practices](https://developer.hashicorp.com/terraform/language/modules)

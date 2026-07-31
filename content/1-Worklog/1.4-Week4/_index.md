---
title: "Worklog Week 4"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

- **Start Date:** June 22, 2026
- **Completion Date:** June 27, 2026

### Objectives for Week 4:
- Theoretical research on High Availability solutions with Application Load Balancers (ALB) and Auto Scaling Groups (ASG)
- Study SSL certificate management and issuance using AWS Certificate Manager (ACM)
- Hands-on deployment of ALB fronting EC2 Web Servers, configure CPU-based Auto Scaling policies, and execute Stress Testing

### Tasks to implement this week:
| Day | Task |
| --- | --- |
| 2 | Study Application Load Balancer (ALB) concepts: Target Groups, Health Checks, Path-based Routing Rules, and Round Robin distribution algorithms. |
| 3 | Research Amazon EC2 Auto Scaling Groups (ASG): Launch Templates, Dynamic Target Tracking Scaling Policies (CPU threshold), Min/Max/Desired capacities, and High Availability topology. |
| 4 | Research AWS Certificate Manager (ACM): DNS Validation procedures and binding HTTPS SSL certificates to Application Load Balancers. |
| 5 | Hands-on AWS Lab (ALB & ASG Setup): Build Launch Template, provision ALB distributing traffic to EC2 Web Servers across 2 AZs, and attach ACM SSL certificate. |
| 6 | Configure ASG scaling rule to auto-scale EC2 instances when CPU > 70%, run `stress` utility for load testing validation, and compile Week 4 report. |

### Results achieved in Week 4:
* Completed Week 4 tasks on schedule (High Availability & Auto Scaling Labs).
* Mastered load balancing, auto-scaling principles, and HTTPS SSL security enforcement for cloud applications.

### References & Study Materials:
- [Elastic Load Balancing Application Load Balancers Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)
- [Amazon EC2 Auto Scaling Groups User Guide](https://docs.aws.amazon.com/autoscaling/ec2/userguide/)
- [AWS Certificate Manager (ACM) SSL Certificate User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)

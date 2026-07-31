---
title: "Worklog Week 8"
date: 2026-07-20
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

- **Start Date:** July 20, 2026
- **Completion Date:** July 25, 2026

### Objectives for Week 8:
- Theoretical research on Containerization, Docker Multi-stage Builds & container orchestration on Amazon ECS Fargate Serverless
- Hands-on AWS Lab: Image packaging, registry management on Amazon ECR, and deploying Fargate Services behind ALB
- Complete Docker microservices packaging (React Frontend Nginx & Spring Boot Backend Java 21) and Nginx reverse proxy `/api` configuration for Shopsflow

### Tasks to implement this week:
| Day | Task |
| --- | --- |
| 2 | Study Docker Containerization theory: Packaging application code, image size optimization via Docker Multi-stage Builds, and Serverless Container orchestration on Amazon ECS Fargate. |
| 3 | Research Amazon ECR container registry, Task Definitions configuration (CPU, Memory, Container Definitions, Port Mappings), and ECS Cluster setup. |
| 4 | Hands-on AWS Lab (Docker Container Packaging): Draft Dockerfile for React Frontend (Nginx 1.25) & Spring Boot Backend (Java 21), build and push images to Amazon ECR repositories. |
| 5 | Hands-on AWS Lab (Amazon ECS Fargate Deployment): Create ECS Task Definition, deploy multi-container Fargate Service fronted by Application Load Balancer (ALB). |
| 6 | Configure Nginx reverse proxy routing `/api` to Spring Boot Backend, fine-tune RAM/CPU for Shopsflow Fargate Services, and compile Week 8 report. |

### Results achieved in Week 8:
* Completed Week 8 tasks on schedule (Docker Containerization & ECS Fargate Labs).
* Successfully containerized Shopsflow microservices and operated stably on ECS Fargate Serverless Containers.

### References & Study Materials:
- [Amazon ECS Fargate User Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html)
- [Amazon ECR User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/)
- [Docker Multi-Stage Build Guide](https://docs.docker.com/build/building/multi-stage/)

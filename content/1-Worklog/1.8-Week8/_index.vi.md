---
title: "Worklog Tuần 8"
date: 2026-07-20
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

- **Ngày bắt đầu:** 20/07/2026
- **Ngày hoàn thành:** 25/07/2026

### Mục tiêu tuần 8:
- Nghiên cứu lý thuyết Containerization, Docker Multi-stage Builds & điều phối container trên Amazon ECS Fargate Serverless
- Thực hành bài Lab AWS: Đóng gói Docker Images, lưu trữ trên Amazon ECR và điều phối Fargate Services đằng sau ALB
- Hoàn thiện đóng gói Docker microservices (React Frontend Nginx & Spring Boot Backend Java 21) và cấu hình Nginx reverse proxy `/api` cho dự án Shopsflow

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc |
| --- | --- |
| 2 | Tìm hiểu lý thuyết Docker Containerization: Đóng gói ứng dụng, tối ưu kích thước image bằng Docker Multi-stage Builds và kiến trúc điều phối Serverless Container Amazon ECS Fargate. |
| 3 | Nghiên cứu dịch vụ lưu trữ container Amazon ECR, cấu hình Task Definitions (CPU, Memory, Container Definitions, Port Mappings) và ECS Cluster. |
| 4 | Thực hành bài Lab AWS (Docker Container Packaging): Soạn Dockerfile cho React Frontend (Nginx 1.25) & Spring Boot Backend (Java 21), build và push images lên Amazon ECR repository. |
| 5 | Thực hành bài Lab AWS (Amazon ECS Fargate Deployment): Khởi tạo ECS Task Definition, deploy Fargate Service chạy đa container đằng sau Application Load Balancer (ALB). |
| 6 | Cấu hình Nginx reverse proxy định tuyến `/api` về Spring Boot Backend, tinh chỉnh RAM/CPU cho Fargate Services của Shopsflow và tổng hợp báo cáo tuần 8. |

### Kết quả đạt được tuần 8:
* Hoàn thành đúng tiến độ công việc tuần 8 (Docker Containerization & ECS Fargate Labs).
* Đóng gói thành công microservices của Shopsflow và vận hành ổn định trên Serverless Containers ECS Fargate.

### Nguồn tài liệu hướng dẫn tham khảo:
- [Amazon ECS Fargate User Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html)
- [Amazon ECR User Guide](https://docs.aws.amazon.com/AmazonECR/latest/userguide/)
- [Docker Multi-Stage Build Guide](https://docs.docker.com/build/building/multi-stage/)

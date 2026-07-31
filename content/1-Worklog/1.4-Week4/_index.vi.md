---
title: "Worklog Tuần 4"
date: 2026-06-22
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

- **Ngày bắt đầu:** 22/06/2026
- **Ngày hoàn thành:** 27/06/2026

### Mục tiêu tuần 4:
- Nghiên cứu giải pháp chịu tải High Availability với Application Load Balancer (ALB) và Auto Scaling Groups (ASG)
- Tìm hiểu quy trình quản lý và đăng ký chứng chỉ bảo mật HTTPS SSL với AWS Certificate Manager (ACM)
- Thực hành triển khai ALB đằng sau 2 máy chủ EC2 Web Servers, thiết lập luật co giãn tự động theo CPU load và chạy bài kiểm thử chịu tải Stress Test

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc |
| --- | --- |
| 2 | Nghiên cứu lý thuyết Application Load Balancer (ALB): Cấu trúc Target Groups, Health Checks, Routing Rules theo URL Path và thuật toán điều hướng Round Robin. |
| 3 | Tìm hiểu Amazon EC2 Auto Scaling Groups (ASG): Launch Templates, Dynamic Scaling Policies (Target Tracking theo CPU), Min/Max/Desired Capacity và khái niệm High Availability. |
| 4 | Nghiên cứu dịch vụ AWS Certificate Manager (ACM): Cơ chế xác thực tên miền DNS Validation và tích hợp chứng chỉ HTTPS SSL vào bộ cân bằng tải ALB. |
| 5 | Thực hành bài Lab AWS (ALB & ASG Setup): Tạo Launch Template, khởi tạo ALB phân phối lưu lượng vào 2 EC2 Web Servers ở 2 AZs khác nhau và gắn chứng chỉ SSL ACM. |
| 6 | Cấu hình Auto Scaling policy tự động tăng máy chủ khi CPU > 70%, sử dụng công cụ `stress` giả lập lưu lượng cao kiểm thử co giãn và hoàn thiện báo cáo tuần 4. |

### Kết quả đạt được tuần 4:
* Hoàn thành đúng tiến độ công việc tuần 4 (High Availability & Auto Scaling Labs).
* Hiểu sâu nguyên lý cân bằng tải, co giãn tự động và bảo mật HTTPS SSL cho ứng dụng điện toán đám mây.

### Nguồn tài liệu hướng dẫn tham khảo:
- [Elastic Load Balancing Application Load Balancers Guide](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/)
- [Amazon EC2 Auto Scaling Groups User Guide](https://docs.aws.amazon.com/autoscaling/ec2/userguide/)
- [AWS Certificate Manager (ACM) SSL Certificate User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)

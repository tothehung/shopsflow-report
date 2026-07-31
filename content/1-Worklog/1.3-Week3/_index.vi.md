---
title: "Worklog Tuần 3"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

- **Ngày bắt đầu:** 15/06/2026
- **Ngày hoàn thành:** 20/06/2026

### Mục tiêu tuần 3:
- Nghiên cứu hạ tầng mạng ảo Custom VPC, địa chỉ IP CIDR, Public/Private Subnets và Internet Gateway
- Tìm hiểu cơ chế tường lửa Stateful Security Groups vs Stateless Network ACLs (NACLs) & VPC Flow Logs
- Thực hành xây dựng Custom VPC Multi-AZ, triển khai NAT Gateway cho Private Subnet và Bastion Host kết nối SSH an toàn

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc |
| --- | --- |
| 2 | Tìm hiểu lý thuyết Amazon VPC: Quy hoạch dải IP CIDR Block (`10.0.0.0/16`), khái niệm Public Subnet, Private Subnet, Route Tables và Internet Gateway (IGW). |
| 3 | Nghiên cứu cơ chế kiểm soát an ninh mạng: Stateful Security Groups (tường lửa cấp máy chủ EC2) vs Stateless Network ACLs (tường lửa cấp Subnet) và quy tắc Inbound/Outbound. |
| 4 | Thực hành bài Lab AWS (Custom VPC Multi-AZ Architecture): Khởi tạo Custom VPC, chia 2 Public Subnets & 2 Private Subnets nằm trên 2 Availability Zones khác nhau. |
| 5 | Thực hành bài Lab AWS (NAT Gateway & Bastion Host): Khởi tạo Elastic IP (EIP), cấu hình NAT Gateway hỗ trợ Private Subnets kết nối Internet và dựng SSH Bastion Host. |
| 6 | Kích hoạt VPC Flow Logs đẩy nhật ký mạng về CloudWatch Log Groups, kiểm thử truy cập an toàn SSH và tổng hợp báo cáo thu hoạch tuần 3. |

### Kết quả đạt được tuần 3:
* Hoàn thành đúng tiến độ công việc tuần 3 (Networking & Custom VPC Architecture Labs).
* Tự tay thiết kế và khởi tạo thành công hạ tầng mạng ảo VPC Multi-AZ an toàn theo tiêu chuẩn AWS.

### Nguồn tài liệu hướng dẫn tham khảo:
- [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)
- [AWS Security Groups & Network ACLs Guide](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html)
- [AWS NAT Gateway Setup & Administration](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)
- [AWS VPC Flow Logs Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)

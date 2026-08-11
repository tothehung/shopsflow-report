---
title: "Bản đề xuất"
date: 2026-06-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Shopsflow — Full-Stack E-Commerce Application on AWS
## Giải pháp Điện toán Đám mây AWS Chuẩn Chịu Tải, Bảo Mật & Tự Động Hóa Hạ Tầng

### 1. Tóm tắt điều hành (Executive Summary)
Shopsflow là nền tảng thương mại điện tử Full-Stack hiện đại được thiết kế và triển khai trên hạ tầng Điện toán Đám mây AWS. Hệ thống phục vụ nhu cầu mua sắm trực tuyến của khách hàng (duyệt sản phẩm, giỏ hàng, đặt hàng, thanh toán VNPay Sandbox, upload ảnh Cloudinary, đánh giá sản phẩm) và cung cấp giao diện quản trị chuyên nghiệp cho Admin (quản lý danh mục, sản phẩm, kho hàng, chuyển trạng thái đơn hàng `PENDING → PAID → SHIPPED → DELIVERED` và kiểm duyệt đánh giá Admin Reviews Moderation).

Hạ tầng hệ thống áp dụng các dịch vụ cốt lõi của AWS bao gồm Amazon EC2, Amazon RDS PostgreSQL Multi-AZ, ElastiCache Redis, Amazon CloudWatch Observability, AWS Secrets Manager và tự động hóa 100% bằng mã nguồn Terraform Modules IaC.

---

### 2. Tuyên bố vấn đề (Problem Statement)

#### Vấn đề hiện tại
Các cửa hàng bán lẻ vừa và nhỏ khi vận hành ứng dụng E-Commerce thường gặp các thách thức lớn:
- **Tự vận hành hạ tầng thủ công**: Dễ xảy ra sự cố nghẽn mạng hoặc downtime vào các thời điểm cao điểm mua sắm.
- **Rủi ro mất mát dữ liệu CSDL**: Tự cài đặt CSDL trên VPS đơn lẻ thiếu cơ chế sao lưu tự động và không có tính sẵn sàng cao (High Availability).
- **Rò rỉ thông tin bảo mật**: Lưu trữ mã khóa JWT Secret và DB Credentials trực tiếp trong mã nguồn hoặc file cấu hình không mã hóa.
- **Thiếu khả năng giám sát tập trung**: Khó phát hiện sự cố hệ thống hoặc tràn ngập lưu lượng bất ngờ nếu không có hệ thống log & alert tự động.

#### Giải pháp Đề xuất
Dự án **Shopsflow on AWS** giải quyết triệt để các vấn đề trên thông qua:
- **Kiến trúc Full-Stack hiện đại**: React 19 (Vite, TypeScript, Nginx) ở Frontend và Spring Boot (Java 21, JPA, Flyway Migration) ở Backend.
- **Quản lý CSDL Managed trên AWS**: Sử dụng Amazon RDS PostgreSQL Multi-AZ đảm bảo an toàn dữ liệu và đệm dữ liệu tốc độ cao ElastiCache Redis.
- **Tự động hóa IaC & Bảo mật**: Sử dụng Terraform Modules khởi tạo hạ tầng, lưu trữ bí mật an toàn trên AWS Secrets Manager và quản lý phân quyền IAM Least Privilege.
- **Giám sát & Cảnh báo tức thời**: Cấu hình CloudWatch Dashboards, Log Groups và CloudWatch Alarms gửi email / Slack alert khi hệ thống gặp sự cố.

#### Lợi ích và Hoàn vốn Đầu tư (ROI)
- **Tăng tính sẵn sàng và độ tin cậy**: Giảm 99.9% rủi ro downtime nhờ hạ tầng Multi-AZ và bộ cân bằng tải ALB.
- **Tự động hóa vận hành DevOps**: Tiết kiệm 80% thời gian triển khai hạ tầng nhờ bộ mã Terraform IaC tái sử dụng.
- **Tối ưu chi phí FinOps**: Khởi tạo tài nguyên theo đúng nhu cầu sử dụng, theo dõi chi phí chặt chẽ với AWS Billing Alarms.

---

### 3. Kiến trúc Giải pháp (Solution Architecture)

Hệ thống Shopsflow được thiết kế theo mô hình kiến trúc chuẩn 3 lớp (3-Tier Architecture) trên mạng ảo AWS Custom VPC Multi-AZ:

![Sơ đồ Kiến trúc Hạ tầng Shopsflow trên AWS](/images/2-Proposal/diagram.png)

#### Dịch vụ AWS sử dụng:
- **Amazon EC2**: Chạy ứng dụng web (Nginx Frontend & Spring Boot Backend) trong Public/Private Subnets.
- **Amazon RDS for PostgreSQL**: Lưu trữ dữ liệu quan hệ quản lý trong Private DB Subnets với cơ chế Multi-AZ.
- **Amazon ElastiCache (Redis)**: Đệm dữ liệu danh mục và sản phẩm cải thiện tốc độ truy vấn.
- **Application Load Balancer (ALB)**: Phân phối lưu lượng truy cập và tích hợp SSL Certificate.
- **AWS Secrets Manager**: Lưu trữ an toàn JWT Secret Key và thông tin kết nối CSDL.
- **Amazon CloudWatch**: Thu thập logs tập trung, hiển thị metrics dashboard và kích hoạt alarms.
- **AWS IAM**: Phân quyền EC2 Instance Profile tối thiểu mà không cần lưu Access Keys.
- **AWS S3 & DynamoDB**: Lưu trữ Terraform State File và quản lý State Locking.

---

### 4. Triển khai Kỹ thuật (Technical Implementation)

#### Các giai đoạn triển khai dự án:
1. **Tháng 1 (Tuần 1 - 4)**: Học AWS, thiết kế CSDL, khởi tạo dự án Spring Boot, hoàn thiện Proposal.
2. **Tháng 2 (Tuần 5 - 9)**: Viết API (Cart, Order, Payment), xử lý Concurrency, viết Unit Tests, hoàn thiện tài liệu Blog và đóng gói báo cáo dự án.

#### Yêu cầu kỹ thuật cốt lõi:
- **Backend**: Java 21, Spring Boot 3, Spring Security JWT, Spring Data JPA, Flyway Database Migration, JUnit 5 & Mockito Unit Testing.
- **Frontend**: React 19, TypeScript, Vite, Axios, Nginx 1.25 reverse proxy.
- **Database & Cache**: PostgreSQL 16, Redis 7.
- **Infrastructure as Code**: Terraform v1.5+, AWS Provider, HCL Modules.

---

### 5. Ước tính Ngân sách (Budget Estimation)

Bảng ước tính chi phí dưới đây được tính dựa trên bảng giá **AWS Singapore Region (`ap-southeast-1`)** cho một hệ thống Shopsflow chuẩn Production. Tất cả chi phí được tính theo tháng cho cấu hình chạy 24/7.

| Dịch vụ AWS | Cấu hình | Chi phí ước tính (USD/tháng) |
|---|---|---|
| **Amazon EC2** (Backend) | 2× `t3.micro` trong ASG (Private Subnet) | ~$15.18 |
| **Amazon RDS PostgreSQL** | `db.t3.micro` Multi-AZ, 20GB GP3 storage | ~$28.62 |
| **Application Load Balancer** | 1 ALB (~10 LCU/tháng) | ~$10.08 |
| **Amazon CloudFront** | 10GB data transfer out, 1 triệu requests | ~$1.20 |
| **Amazon S3** | Frontend static assets + DB backups ~5GB | ~$0.23 |
| **NAT Gateway** | 1 NAT GW + ~10GB data processed | ~$5.40 |
| **AWS Secrets Manager** | 3 secrets × $0.40 | ~$1.20 |
| **Amazon CloudWatch** | Custom metrics + 5GB logs + 3 Alarms | ~$3.50 |
| **AWS WAF** | 1 Web ACL + 10 triệu requests | ~$6.00 |
| **AWS KMS** | 1 CMK key + 10K API calls | ~$1.03 |
| **Tổng ước tính** | | **~$72.44 / tháng** |

> **💡 Lưu ý AWS Free Tier:** Trong phạm vi bài thực tập và demo, hầu hết các dịch vụ đều nằm trong hạn mức AWS Free Tier (EC2 `t2.micro` 750 giờ/tháng, S3 5GB, CloudWatch 5GB logs, 10 alarms). Chi phí thực tế ước tính cho tài khoản ngoài Free Tier là khoảng **$72.44/tháng**.

- **Quản lý chi phí:** Đặt CloudWatch Billing Alarm nhận thông báo email ngay khi chi phí ước tính vượt mốc **$5 USD**, đồng thời tự động dọn dẹp toàn bộ tài nguyên sau khi nghiệm thu (FinOps Best Practices).



---

### 6. Đánh giá Rủi ro (Risk Assessment)

| Rủi ro | Mức độ | Biện pháp giảm thiểu |
| --- | --- | --- |
| **Rò rỉ secret key / DB password** | Cao | Lưu trữ bí mật trên AWS Secrets Manager & IAM Role, không commit file `.env` lên GitHub. |
| **CSDL RDS bị truy cập trái phép từ Internet** | Cao | Đặt RDS trong Private DB Subnet, chỉ cho phép Security Group của EC2 truy cập vào cổng 5432. |
| **Nghẽn mạng hoặc quá tải CPU** | Trung bình | Đặt ALB phân phối lưu lượng và cấu hình CloudWatch Metric Alarms cảnh báo quá tải. |
| **Vượt quá ngân sách tài khoản AWS** | Thấp | Cấu hình CloudWatch Billing Alarm và xóa tài nguyên dư thừa ngay sau buổi nghiệm thu. |

---

### 7. Kết quả Kỳ vọng (Expected Outcomes)

1. **Hệ thống E-Commerce vận hành hoàn chỉnh trên Internet**: Khách hàng mua sắm, thanh toán VNPay thành công; Admin quản lý đơn hàng và duyệt đánh giá sản phẩm.
2. **Hạ tầng tự động hóa chuẩn Cloud & DevOps**: 100% tài nguyên đám mây được quản lý bằng mã nguồn Terraform Modules có thể tái sử dụng.
3. **Báo cáo Thực tập Tốt nghiệp Chất lượng cao**: Hoàn thiện bộ tài liệu báo cáo thực tập tổng hợp Capstone Report kèm nhật ký 9 tuần chi tiết.
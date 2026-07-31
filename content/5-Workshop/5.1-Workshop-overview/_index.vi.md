---
title: "Giới thiệu & Kiến trúc"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

### 1. Ý tưởng & Mục tiêu dự án

#### Bối cảnh & Bài toán
Hệ thống **Shopsflow** là ứng dụng thương mại điện tử full-stack hoàn chỉnh bao gồm giao diện Khách hàng (Storefront) để tìm kiếm, mua sắm sản phẩm và thanh toán trực tuyến qua cổng **VNPay Sandbox**, kết hợp với giao diện Quản trị viên (Admin Portal) để quản lý danh mục sản phẩm, theo dõi đơn hàng và duyệt đánh giá.

Hệ thống được thiết kế dành cho các doanh nghiệp bán lẻ đang muốn chuyển đổi số lên hạ tầng AWS Cloud với chi phí tối ưu, sẵn sàng cao và tự chủ hoàn toàn về mã nguồn cũng như cơ sở dữ liệu.

Hệ thống giải quyết các vấn đề thực tế:
* **Giảm downtime và xung đột môi trường:** Sử dụng công nghệ container hóa (Docker) giúp đảm bảo sự đồng nhất giữa môi trường phát triển và máy chủ Cloud.
* **Bảo mật dữ liệu nghiêm ngặt:** Cô lập hoàn toàn máy chủ Backend API và Cơ sở dữ liệu trong vùng mạng riêng (Private Subnets), ngăn chặn triệt để mọi truy cập trực tiếp từ Internet công cộng.
* **Bảo toàn dữ liệu:** Tự động hóa quy trình sao lưu (backup) cơ sở dữ liệu PostgreSQL định kỳ qua kết nối mạng nội bộ của AWS (VPC Gateway Endpoint), tránh rủi ro mất mát dữ liệu.
* **Giám sát tập trung:** Tập trung hóa toàn bộ log ứng dụng và metrics máy chủ lên CloudWatch để chủ động phát hiện sự cố.

#### Mục tiêu cụ thể (Outputs)
* **Frontend Web:** React + Vite Single Page Application (SPA) được deploy tĩnh trên Amazon S3 và phân phối toàn cầu qua Amazon CloudFront CDN.
* **Backend API:** Spring Boot RESTful API chạy trên EC2 đặt trong Private Subnet, quản lý tự động bởi Auto Scaling Group (ASG) phía sau Application Load Balancer (ALB).
* **Database RDS:** PostgreSQL Database chạy ở chế độ Multi-AZ Standby, tắt hoàn toàn khả năng truy cập công cộng (`Publicly Accessible: No`).
* **Security & Encryption:** Lưu trữ mật khẩu và JWT secret bằng AWS Secrets Manager mã hóa bởi KMS Key. Bảo vệ CloudFront bằng tường lửa AWS WAF.
* **Monitoring & Backup:** Dashboard giám sát và logs tập trung trên CloudWatch. Backup RDS PostgreSQL nén đẩy lên S3 qua VPC Gateway Endpoint.

#### Phù hợp chương trình First Cloud AI Journey (FCAJ)
Dự án áp dụng các dịch vụ cốt lõi của AWS bao gồm: **VPC**, **EC2**, **RDS**, **CloudFront**, **WAF**, **S3**, **Secrets Manager**, **KMS**, **CloudWatch** và **IAM**. Cấu trúc hạ tầng tuân thủ nguyên tắc bảo mật và sẵn sàng cao của AWS (Well-Architected Framework).

---

### 2. Sơ đồ kiến trúc & Thiết kế kỹ thuật

#### Sơ đồ kiến trúc (Architecture Diagram)

Dưới đây là sơ đồ kiến trúc mô tả cấu trúc phân tầng và luồng dữ liệu của ứng dụng Shopsflow triển khai trên AWS:

![Architecture Diagram](/images/5-Workshop/5.1-Workshop-overview/diagram1.jpg)

#### Lựa chọn dịch vụ (Service Selection Rationale)

* **Amazon CloudFront & Amazon S3 (Frontend):**
  * Đưa web tĩnh lên S3 và phân phối qua CloudFront giúp giảm tải cho EC2, tăng tốc độ truy cập toàn cầu nhờ đệm Edge Locations và tiết kiệm chi phí. Bảo vệ bởi AWS WAF ngăn chặn tấn công biên.
* **AWS ALB & Auto Scaling Group (EC2 Backend):**
  * ALB phân phối lưu lượng truy cập tới các EC2 instances trong Private Subnet của 2 Availability Zones (AZ), đảm bảo tính sẵn sàng cao. Auto Scaling Group tự động co giãn EC2 theo mức độ sử dụng CPU.
* **Amazon RDS PostgreSQL Multi-AZ:**
  * Tự động nhân bản dữ liệu sang một vùng dự phòng Standby ở AZ2. Khi AZ chính gặp sự cố, RDS tự động failover sang Standby database mà không làm gián đoạn ứng dụng.
* **AWS Secrets Manager & KMS:**
  * Lưu trữ tập trung các thông tin nhạy cảm (Database Password, JWT Secret) được mã hóa bằng KMS Key. EC2 tự động lấy credentials tạm thời lúc boot.
* **VPC Gateway Endpoint (S3):**
  * Cho phép EC2 trong Private Subnet đẩy tệp backup cơ sở dữ liệu lên S3 qua mạng nội bộ AWS, không đi qua NAT Gateway giúp tiết kiệm chi phí data processing.

#### Phân tầng bảo mật (Security Chain)
Security Groups được thiết lập theo chuỗi kiểm soát chặt chẽ:
`Internet` → `CloudFront (WAF)` → `ALB SG (Port 80/443)` → `EC2 SG (Port 8080 từ ALB SG)` → `RDS SG (Port 5432 từ EC2 SG)`.

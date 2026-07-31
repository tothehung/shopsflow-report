---
title: "Dọn dẹp tài nguyên"
date: 2026-06-15
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Kiến trúc Doanh nghiệp sẵn sàng cao (Enterprise HA) sử dụng các dịch vụ có phí duy trì theo giờ (như NAT Gateway, Application Load Balancer). Hãy chắc chắn thực hiện dọn dẹp tài nguyên theo thứ tự dưới đây để tránh các chi phí phát sinh ngoài mong muốn trên tài khoản AWS của bạn.

> ⚠️ **Lưu ý quan trọng:** Thực hiện dọn dẹp tài nguyên theo đúng thứ tự liệt kê để tránh lỗi phụ thuộc (dependency errors).

---

### Các bước dọn dẹp tài nguyên theo thứ tự

#### 1. Xóa CloudFront Distribution & WAF Web ACL
1. Truy cập **AWS Console** → **CloudFront** → Chọn distribution `shopsflow` → Click **Disable**.
2. Đợi trạng thái chuyển sang *Disabled* (~5 phút) → Chọn distribution và click **Delete**.
3. Truy cập **AWS WAF & Shield** → **Web ACLs** → Hủy liên kết CloudFront distribution → Click **Delete** Web ACL `shopsflow-waf`.

#### 2. Xóa Application Load Balancer (ALB) & Target Group
1. Truy cập **EC2** → **Load Balancers** → Chọn `shopsflow-alb` → Click **Actions** → **Delete load balancer**.
2. Truy cập **Target Groups** → Chọn `shopsflow-tg` → Click **Actions** → **Delete**.

#### 3. Xóa Auto Scaling Group (ASG) & Launch Template
1. Truy cập **EC2** → **Auto Scaling Groups** → Chọn `shopsflow-asg` → Click **Delete** (gõ `delete` để xác nhận).
   * *Lưu ý:* Việc xóa ASG sẽ tự động hủy (Terminate) tất cả EC2 instances trong Private Subnets.
2. Truy cập **Launch Templates** → Chọn `shopsflow-lt` → Click **Actions** → **Delete template**.

#### 4. Xóa Database RDS PostgreSQL Multi-AZ
1. Truy cập **RDS** → **Databases** → Chọn `shopsflow-postgres` → Click **Actions** → **Delete**.
2. Bỏ chọn *Create final snapshot?*, gõ `delete me` để xác nhận xóa.
3. Sau khi database được xóa hoàn toàn, truy cập **Subnet groups** → Xóa `shopsflow-db-subnet-group`.

#### 5. Xóa các S3 Buckets
1. Truy cập **S3** → Chọn và làm rỗng (**Empty**) rồi xóa (**Delete**) lần lượt 2 buckets:
   * Bucket frontend: `shopsflow-frontend-<your-account-id>`
   * Bucket backup database: `shopsflow-db-backups-<your-account-id>`

![Dọn dẹp và xóa các S3 Buckets](/images/5-Workshop/5.6-Cleanup/delete-s3.png)

#### 6. Xóa Secrets Manager & KMS Key
1. Truy cập **Secrets Manager** → Chọn secret `shopsflow/production/secrets` → Actions → **Delete secret** (đặt thời gian phục hồi 7 ngày).
2. Truy cập **KMS** → Chọn key `shopsflow-kms-key` → Actions → **Schedule key deletion** (7 ngày).

#### 7. Giải phóng tài nguyên Mạng (NAT Gateway & VPC)
```bash
# 1. Xóa NAT Gateway (~2 phút)
aws ec2 delete-nat-gateway --nat-gateway-id <NAT_GATEWAY_ID>

# 2. Giải phóng Elastic IP
aws ec2 release-address --allocation-id <ELASTIC_IP_ALLOC_ID>

# 3. Xóa S3 VPC Endpoint
aws ec2 delete-vpc-endpoints --vpc-endpoint-ids <ENDPOINT_ID>

# 4. Tách và xóa Internet Gateway
aws ec2 detach-internet-gateway --internet-gateway-id <IGW_ID> --vpc-id <VPC_ID>
aws ec2 delete-internet-gateway --internet-gateway-id <IGW_ID>

# 5. Xóa Subnets, Route Tables, Security Groups và VPC
aws ec2 delete-vpc --vpc-id <VPC_ID>
```

#### 8. Xóa IAM Role & CloudWatch Resources
1. Xóa IAM Role `ShopsflowEC2Role`.
2. Xóa CloudWatch Dashboard `ShopsflowDashboard`, các Alarms và Log Group `/shopsflow/application`.

---

### Tự đánh giá & Bài học rút ra (Reflection)

#### 1. Khó khăn gặp phải
* **Độ phức tạp của hạ tầng mạng Multi-AZ:** Việc chia VPC thành 6 subnets phân bổ trên 2 Availability Zones yêu cầu cấu hình Route Tables chính xác. Ban đầu, các EC2 instances trong Private Subnets không kết nối được Internet để cài đặt Docker do cấu hình sai tuyến đường ra NAT Gateway.
* **Tích hợp Secrets Manager & KMS:** Cấu hình User Data cho EC2 tự động kéo database credentials và JWT secret về giải mã dễ gặp lỗi nếu IAM Role chưa được gán chính xác Inline Policy cho phép `kms:Decrypt`.
* **Định tuyến CloudFront & CORS:** Cấu hình gộp chung Frontend S3 và Backend ALB dưới một distribution CloudFront với đường dẫn `/api/*` đòi hỏi thiết lập Cache Policy và Origin Request Policy chuẩn xác để tránh lỗi CORS.

#### 2. Bài học rút ra
* **Thấu hiểu thực tế kiến trúc High Availability (HA):** Nắm vững cơ chế hoạt động thực tế của ALB và ASG trong việc phân chia lưu lượng, tự động phát hiện và thay thế máy chủ lỗi (Self-healing).
* **Bảo mật chuyên sâu (Defense in Depth):** Hiểu rõ tầm quan trọng của việc cô lập hoàn toàn cơ sở dữ liệu và ứng dụng backend trong Private Subnet, chỉ cho phép traffic đi qua các chốt chặn được kiểm soát (WAF, CloudFront, ALB).
* **Tối ưu hóa chi phí với VPC Endpoint:** Học cách sử dụng Gateway VPC Endpoint để truyền tải dữ liệu dung lượng lớn (Backup database) lên S3 qua mạng nội bộ mà không đi qua NAT Gateway, tránh phát sinh chi phí băng thông NAT rất đắt đỏ.

#### 3. Hướng phát triển trong tương lai
* **Infrastructure as Code (IaC):** Sử dụng các công cụ IaC như **Terraform** để định nghĩa toàn bộ hạ tầng dưới dạng mã nguồn, giúp triển khai tự động, nhất quán và tránh sai sót thủ công.
* **Triển khai Serverless/Container nâng cao:** Hướng tới đưa Spring Boot Backend lên chạy trên **AWS ECS Fargate** để loại bỏ hoàn toàn việc quản trị máy chủ EC2.
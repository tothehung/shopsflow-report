---
title: "Triển khai Spring Boot Backend API"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

Ở bước này, bạn sẽ tạo **Application Load Balancer (ALB)**, **EC2 Launch Template** tự động khởi chạy Shopsflow Spring Boot API trong Docker, và **Auto Scaling Group (ASG)** để đảm bảo tính sẵn sàng cao trên hai private subnets.

---

### 1. Tạo ALB Target Group

1. Truy cập **EC2** → **Target Groups** → **Create target group**.

| Trường                | Giá trị            |
| --------------------- | ------------------ |
| Target type           | Instances          |
| Target group name     | `shopsflow-tg`     |
| Protocol              | HTTP               |
| Port                  | `8080`             |
| VPC                   | `shopsflow-vpc`    |
| Health check protocol | HTTP               |
| Health check path     | `/actuator/health` |
| Healthy threshold     | `2`                |
| Unhealthy threshold   | `3`                |
| Timeout               | `5 giây`           |
| Interval              | `30 giây`          |

2. Click **Next** → **Create target group** (chưa đăng ký instance nào).

---

### 2. Tạo Application Load Balancer

1. Truy cập **EC2** → **Load Balancers** → **Create load balancer** → **Application Load Balancer**.

| Trường          | Giá trị                                                  |
| --------------- | -------------------------------------------------------- |
| Name            | `shopsflow-alb`                                          |
| Scheme          | Internet-facing                                          |
| IP address type | IPv4                                                     |
| VPC             | `shopsflow-vpc`                                          |
| Subnets         | `shopsflow-public-1` (AZ-a), `shopsflow-public-2` (AZ-b) |
| Security groups | `shopsflow-alb-sg`                                       |

2. Tại mục **Listeners**:
   - Thêm listener: Protocol `HTTP`, Port `80`
   - Default action: Forward to `shopsflow-tg`

3. Click **Create load balancer**.

![ALB shopsflow-alb được tạo thành công (Internet-facing, Active)](/images/5-Workshop/5.3-S3-vpc/alb-created.jpg)

{{% notice note %}}
Sao chép **DNS name** của ALB (vd: `shopsflow-alb-123456.ap-southeast-1.elb.amazonaws.com`) — bạn sẽ cần nó khi build frontend ở Mục 5.4.
{{% /notice %}}

---

### 3. Chuẩn bị EC2 User Data Script

EC2 sẽ tự động lấy credentials từ **Secrets Manager** và khởi động container Backend Shopsflow khi boot.

Thay `<RDS_ENDPOINT>` bằng endpoint đã sao chép ở bước 5.3.1.

```bash
#!/bin/bash
yum update -y
yum install -y docker aws-cli postgresql15
systemctl start docker
systemctl enable docker
usermod -aG docker ec2-user

# Đợi mạng ổn định
sleep 10

# Lấy toàn bộ secrets từ Secrets Manager
SECRETS=$(aws secretsmanager get-secret-value \
  --secret-id shopsflow/production/secrets \
  --region ap-southeast-1 \
  --query SecretString --output text)

export DB_URL=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['SPRING_DATASOURCE_URL'])")
export DB_USER=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['SPRING_DATASOURCE_USERNAME'])")
export DB_PASS=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['SPRING_DATASOURCE_PASSWORD'])")
export JWT_SECRET=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['JWT_SECRET'])")
export VNPAY_HASH=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['VNPAY_HASH_SECRET'])")

# Khởi động container Shopsflow Backend
docker run -d \
  --name shopsflow-backend \
  --restart always \
  -p 8080:8080 \
  -e SPRING_DATASOURCE_URL="$DB_URL" \
  -e SPRING_DATASOURCE_USERNAME="$DB_USER" \
  -e SPRING_DATASOURCE_PASSWORD="$DB_PASS" \
  -e JWT_SECRET="$JWT_SECRET" \
  -e VNPAY_HASH_SECRET="$VNPAY_HASH" \
  -e SPRING_PROFILES_ACTIVE=production \
  your-dockerhub-username/shopsflow-backend:latest

echo "Shopsflow Backend started successfully" >> /var/log/shopsflow-init.log
```

---

### 4. Tạo EC2 Launch Template

1. Truy cập **EC2** → **Launch Templates** → **Create launch template**.

| Trường               | Giá trị                      |
| -------------------- | ---------------------------- |
| Name                 | `shopsflow-lt`               |
| AMI                  | Amazon Linux 2023 (mới nhất) |
| Instance type        | `t3.small`                   |
| Key pair             | SSH key pair của bạn         |
| Security groups      | `shopsflow-ec2-sg`           |
| IAM instance profile | `ShopsflowEC2Role`           |

2. Tại mục **Advanced details** → **User data**: Dán script từ Bước 3.
3. Click **Create launch template**.

---

### 5. Tạo Auto Scaling Group

1. Truy cập **EC2** → **Auto Scaling Groups** → **Create Auto Scaling group**.

| Trường          | Giá trị                                              |
| --------------- | ---------------------------------------------------- |
| Name            | `shopsflow-asg`                                      |
| Launch template | `shopsflow-lt`                                       |
| VPC             | `shopsflow-vpc`                                      |
| Subnets         | `shopsflow-private-app-1`, `shopsflow-private-app-2` |

2. Tại mục **Load balancing** → Attach to existing load balancer → Chọn `shopsflow-tg`.
3. Tại mục **Health checks** → Bật ELB health checks.
4. Tại mục **Group size**:

|                  | Giá trị |
| ---------------- | ------- |
| Desired capacity | `2`     |
| Minimum capacity | `1`     |
| Maximum capacity | `4`     |

5. Tại mục **Automatic scaling** → Thêm **Target tracking scaling policy**:
   - Metric: Average CPU utilization
   - Target value: `60`

6. Click **Create Auto Scaling group**.

---

### 6. Xác nhận Backend hoạt động

Sau khoảng **3–5 phút**, ASG sẽ khởi chạy các EC2 instances. Kiểm tra backend hoạt động bình thường:

```bash
# Kiểm tra qua ALB DNS
curl http://<ALB_DNS_NAME>/actuator/health
# Kết quả mong đợi: {"status":"UP"}

# Kiểm tra log Flyway migration trên EC2
aws ssm start-session --target <INSTANCE_ID>
docker logs shopsflow-backend 2>&1 | grep -i flyway
# Kết quả mong đợi:
# Successfully applied 12 migrations to schema "public"
```

Truy cập **EC2** → **Target Groups** → `shopsflow-tg` → tab **Targets** và xác nhận tất cả targets hiển thị trạng thái **Healthy**.

**Spring Boot Backend đã chạy trong cấu hình sẵn sàng cao trên hai Availability Zones, phía sau Application Load Balancer.**

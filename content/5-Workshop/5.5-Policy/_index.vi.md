---
title: "Giám sát & Backup"
date: 2026-06-15
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

Bài viết này hướng dẫn cấu hình giám sát tập trung qua **Amazon CloudWatch**, thiết lập tự động sao lưu cơ sở dữ liệu riêng tư qua **VPC Gateway Endpoint**, và các kịch bản kiểm thử (Validation Scenarios) kiểm chứng khả năng tự động co giãn (Auto Scaling) và phân tải của hệ thống.

---

### 1. Cấu hình CloudWatch Log Groups & Agent

Các máy chủ EC2 tự động cài đặt và khởi chạy CloudWatch Agent thông qua User Data script để thu thập log container Docker và metrics hệ thống.

![Sơ đồ Kiến trúc Giám sát CloudWatch & Gateway VPC Endpoint Backup](/images/5-Workshop/5.5-Policy/VPCEndpointPolicyDiagram.png)

#### Cài đặt và kích hoạt CloudWatch Agent
```bash
# Kết nối EC2 qua Session Manager hoặc SSH
sudo yum install -y amazon-cloudwatch-agent

# Khởi động agent với file cấu hình tập trung
sudo systemctl start amazon-cloudwatch-agent
sudo systemctl enable amazon-cloudwatch-agent
```

---

### 2. Tạo CloudWatch Alarms & SNS Notification

#### Alarm 1: Cảnh báo CPU Backend quá cao (`shopsflow-asg-high-cpu`)
1. Truy cập **CloudWatch** → **Alarms** → **Create alarm**.
2. **Metric:** `EC2` → `By Auto Scaling Group` → Chọn `CPUUtilization` của `shopsflow-asg`.
3. **Threshold:** Static, Greater than `80%` (vượt 80% trong 2 chu kỳ 5 phút).
4. **Action:** Gửi thông báo đến **SNS Topic** `shopsflow-alerts` (đã đăng ký email cá nhân).

#### Alarm 2: Cảnh báo RDS thiếu dung lượng lưu trữ (`shopsflow-rds-low-storage`)
1. **Metric:** `RDS` → `Per-Database Metrics` → Chọn `FreeStorageSpace` của `shopsflow-postgres`.
2. **Threshold:** Less than `2 GB` (2,147,483,648 bytes).
3. **Action:** Gửi thông báo đến SNS Topic `shopsflow-alerts`.

![Bước 14: Cấu hình Alarms và Dashboards Giám sát CloudWatch](/images/5-Workshop/15.jpg)

#### Alarm 3: Cảnh báo chi phí AWS (`shopsflow-billing-alert`)
1. **Metric:** `Billing` → `EstimatedCharges` → Currency: `USD`.
2. **Threshold:** Greater than `$5 USD`.
3. **Action:** Gửi thông báo đến SNS Topic `shopsflow-alerts`.

---

### 3. Tự động hóa Backup RDS PostgreSQL lên S3 qua VPC Gateway Endpoint

Với **VPC Gateway Endpoint** đã thiết lập, dữ liệu nén dump từ PostgreSQL trên EC2 được truyền lên S3 hoàn toàn qua mạng riêng của AWS, bảo mật và không tốn chi phí NAT Gateway data transfer.

#### Tạo Script Backup tự động (`/opt/shopsflow-backup.sh`)
```bash
#!/bin/bash
DATE=$(date +%Y-%m-%d-%H%M%S)
BACKUP_FILE="/tmp/shopsflow-backup-${DATE}.sql.gz"
S3_BUCKET="shopsflow-db-backups-<your-account-id>"

# Lấy credentials từ Secrets Manager
RDS_HOST="<RDS_ENDPOINT>"
DB_PASS=$(aws secretsmanager get-secret-value \
  --secret-id shopsflow/production/secrets \
  --query SecretString --output text | python3 -c \
  "import sys, json; print(json.load(sys.stdin)['SPRING_DATASOURCE_PASSWORD'])")

# Dump và nén cơ sở dữ liệu
PGPASSWORD="$DB_PASS" pg_dump \
  -h "$RDS_HOST" \
  -U shopsflow_admin \
  -d shopsflow \
  --no-password | gzip > "$BACKUP_FILE"

# Upload lên S3 qua Gateway Endpoint (không qua NAT Gateway)
aws s3 cp "$BACKUP_FILE" "s3://${S3_BUCKET}/backups/" --storage-class STANDARD_IA

# Xóa file tạm
rm -f "$BACKUP_FILE"
echo "[$(date)] Backup completed: ${BACKUP_FILE}"
```

#### Xác nhận kết nối riêng tư (VPC Endpoint Check)
```bash
# Thực thi backup thủ công
sudo /opt/shopsflow-backup.sh

# Kiểm tra file trên S3
aws s3 ls s3://shopsflow-db-backups-<your-account-id>/backups/ --human-readable
```

**Kết quả mong đợi:** File `.sql.gz` xuất hiện trong S3 bucket mà không đi qua NAT Gateway.

---

### 4. Các kịch bản kiểm thử hệ thống (Validation Scenarios)

#### 🧪 Kịch bản 1: Kiểm thử Định tuyến Edge-to-Backend
* **Thao tác:** Truy cập URL CloudFront Distribution (vd: `https://dxxxxx.cloudfront.net`) trên trình duyệt.
* **Kết quả mong đợi:**
  * Trang chủ React Frontend tải thành công từ S3 qua CloudFront CDN.
  * Khi click duyệt sản phẩm, CloudFront định tuyến chính xác request `/api/*` tới ALB. ALB phân tải tới các EC2 instances trong Private Subnet để lấy dữ liệu từ RDS PostgreSQL.

#### 🧪 Kịch bản 2: Kiểm tra Log Streams tập trung trên CloudWatch
* **Thao tác:** Truy cập **CloudWatch** → **Log groups** → Chọn `/shopsflow/application`.
* **Kết quả mong đợi:** Xuất hiện đầy đủ các Log Streams từ các EC2 instances trong Auto Scaling Group. Log khởi động của Spring Boot và Flyway migrations hiển thị rõ ràng.

#### 🧪 Kịch bản 3: Kiểm thử Trừ kho đồng thời (Concurrency Checkout)
* **Thao tác:** Sử dụng Apache Benchmark (`ab`) gửi đồng thời 10 requests mua 1 sản phẩm cuối cùng:
  ```bash
  ab -n 20 -c 10 -p payload.json -T application/json https://dxxxxx.cloudfront.net/api/checkout
  ```
* **Kết quả mong đợi:**
  * Chỉ duy nhất 1 request thanh toán thành công (HTTP 200). Các request đồng thời còn lại trả về mã lỗi `409 Conflict`.
  * Log của Spring Boot ghi nhận `OptimisticLockingFailureException`. Số lượng tồn kho trong database RDS không bị âm.

#### 🧪 Kịch bản 4: Giả lập quá tải CPU để kiểm tra Auto Scaling & CloudWatch Alarm
* **Thao tác:** SSH hoặc dùng Session Manager vào một EC2 instance trong Private Subnet và chạy lệnh `stress`:
  ```bash
  sudo yum install -y stress
  stress --cpu 4 --timeout 300s
  ```
* **Kết quả mong đợi:**
  1. Đồ thị **CPU Utilization** của ASG tăng vượt ngưỡng 80%.
  2. CloudWatch Alarm `shopsflow-asg-high-cpu` chuyển trạng thái `OK` → `ALARM`, hệ thống tự động gửi email cảnh báo qua SNS Topic.
  3. **Auto Scaling Group** phát hiện quá tải → Kích hoạt scale-out → Tự động khởi tạo máy chủ EC2 thứ 3.
  4. Máy chủ thứ 3 tự động được đăng ký (Register) vào Target Group `shopsflow-tg` của ALB để chia tải. Sau khi hết 300 giây stress, hệ thống tự động scale-in giảm về 2 instances.

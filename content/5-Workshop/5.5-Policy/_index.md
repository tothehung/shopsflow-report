---
title: "Monitoring & Backup"
date: 2026-06-15
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

This section covers configuring centralized logging and metrics with **Amazon CloudWatch**, automating private database backups via the **VPC Gateway Endpoint**, and executing end-to-end system validation test scenarios.

---

### 1. CloudWatch Log Groups & Agent Configuration

EC2 instances automatically install and run the CloudWatch Agent via User Data to collect Docker container logs and system OS metrics.

![CloudWatch Monitoring & Gateway VPC Endpoint Backup Architecture](/images/5-Workshop/5.5-Policy/VPCEndpointPolicyDiagram.png)

#### Start and Enable CloudWatch Agent
```bash
# Connect to EC2 via Session Manager or SSH
sudo yum install -y amazon-cloudwatch-agent

# Start agent with centralized configuration
sudo systemctl start amazon-cloudwatch-agent
sudo systemctl enable amazon-cloudwatch-agent
```

---

### 2. Create CloudWatch Alarms & SNS Notifications

#### Alarm 1: High Backend CPU Usage (`shopsflow-asg-high-cpu`)
1. Navigate to **CloudWatch** → **Alarms** → **Create alarm**.
2. **Metric:** `EC2` → `By Auto Scaling Group` → Select `CPUUtilization` for `shopsflow-asg`.
3. **Threshold:** Static, Greater than `80%` (for 2 consecutive 5-minute evaluation periods).
4. **Action:** Send notification to **SNS Topic** `shopsflow-alerts` (subscribed to your email).

#### Alarm 2: RDS Low Free Storage (`shopsflow-rds-low-storage`)
1. **Metric:** `RDS` → `Per-Database Metrics` → Select `FreeStorageSpace` for `shopsflow-postgres`.
2. **Threshold:** Less than `2 GB` (2,147,483,648 bytes).
3. **Action:** Send notification to SNS Topic `shopsflow-alerts`.

![Step 14: CloudWatch Alarms & Monitoring Dashboards Configuration](/images/5-Workshop/15.jpg)

#### Alarm 3: AWS Billing Cost Alert (`shopsflow-billing-alert`)
1. **Metric:** `Billing` → `EstimatedCharges` → Currency: `USD`.
2. **Threshold:** Greater than `$5 USD`.
3. **Action:** Send notification to SNS Topic `shopsflow-alerts`.

---

### 3. Automated RDS PostgreSQL Backup to S3 via VPC Gateway Endpoint

With the **VPC Gateway Endpoint** configured, compressed PostgreSQL database dumps are transferred directly over AWS's internal private network, ensuring zero NAT Gateway data processing charges.

#### Automated Backup Script (`/opt/shopsflow-backup.sh`)
```bash
#!/bin/bash
DATE=$(date +%Y-%m-%d-%H%M%S)
BACKUP_FILE="/tmp/shopsflow-backup-${DATE}.sql.gz"
S3_BUCKET="shopsflow-db-backups-<your-account-id>"

# Fetch credentials from Secrets Manager
RDS_HOST="<RDS_ENDPOINT>"
DB_PASS=$(aws secretsmanager get-secret-value \
  --secret-id shopsflow/production/secrets \
  --query SecretString --output text | python3 -c \
  "import sys, json; print(json.load(sys.stdin)['SPRING_DATASOURCE_PASSWORD'])")

# Dump and compress database
PGPASSWORD="$DB_PASS" pg_dump \
  -h "$RDS_HOST" \
  -U shopsflow_admin \
  -d shopsflow \
  --no-password | gzip > "$BACKUP_FILE"

# Upload to S3 via Gateway Endpoint (bypassing NAT Gateway)
aws s3 cp "$BACKUP_FILE" "s3://${S3_BUCKET}/backups/" --storage-class STANDARD_IA

# Clean up temp file
rm -f "$BACKUP_FILE"
echo "[$(date)] Backup completed: ${BACKUP_FILE}"
```

#### Verification (VPC Endpoint Check)
```bash
# Execute manual backup
sudo /opt/shopsflow-backup.sh

# List backup files on S3
aws s3 ls s3://shopsflow-db-backups-<your-account-id>/backups/ --human-readable
```

**Expected:** The `.sql.gz` backup file appears in the S3 bucket without traversing the NAT Gateway.

---

### 4. System Validation Scenarios

#### 🧪 Test Case 1: Edge-to-Backend Routing Verification
* **Action:** Access the CloudFront Distribution URL (e.g., `https://dxxxxx.cloudfront.net`) in a browser.
* **Expected:**
  * React Frontend loads successfully from S3 via CloudFront CDN.
  * Navigating product pages routes API requests (`/api/*`) accurately to the ALB. The ALB load balances requests across EC2 instances in Private Subnets to retrieve data from RDS PostgreSQL.

#### 🧪 Test Case 2: Centralized CloudWatch Log Streams
* **Action:** Navigate to **CloudWatch** → **Log groups** → Select `/shopsflow/application`.
* **Expected:** Active Log Streams appear for all running EC2 instances in the Auto Scaling Group. Spring Boot startup logs and Flyway migration outputs display clearly.

#### 🧪 Test Case 3: Concurrency Checkout & Stock Deduction Test
* **Action:** Use Apache Benchmark (`ab`) to send 10 concurrent checkout requests for the last remaining item in stock:
  ```bash
  ab -n 20 -c 10 -p payload.json -T application/json https://dxxxxx.cloudfront.net/api/checkout
  ```
* **Expected:**
  * Exactly 1 request succeeds with HTTP 200 OK. Remaining concurrent requests return HTTP `409 Conflict`.
  * Spring Boot logs record `OptimisticLockingFailureException`. Product inventory in RDS remains non-negative.

#### 🧪 Test Case 4: CPU Stress Test for Auto Scaling & CloudWatch Alarm
* **Action:** Connect to an EC2 instance in a Private Subnet via Session Manager and execute a CPU stress load:
  ```bash
  sudo yum install -y stress
  stress --cpu 4 --timeout 300s
  ```
* **Expected:**
  1. The ASG **CPUUtilization** metric spikes past 80%.
  2. CloudWatch Alarm `shopsflow-asg-high-cpu` transitions from `OK` to `ALARM`, triggering an email alert via SNS.
  3. The **Auto Scaling Group** detects the breach → Triggers scale-out → Automatically provisions a 3rd EC2 instance.
  4. The 3rd instance automatically registers with the ALB Target Group `shopsflow-tg` to share traffic. Once the 300s stress period ends, the system automatically scales back in to 2 instances.

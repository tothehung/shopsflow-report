---
title: "Thiết lập Mạng, Quyền & Secrets"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

Bài viết này hướng dẫn xây dựng hạ tầng mạng riêng ảo **Amazon VPC Multi-AZ**, cấu hình khóa mã hóa **AWS KMS**, lưu trữ bí mật trên **AWS Secrets Manager**, thiết lập **Security Groups** phân tầng và phân quyền **IAM Role**.

---

### 1. Chuẩn bị (Prerequisites)

* **Tài khoản AWS:** Có quyền quản trị (Administrator access).
* **AWS Region:** Chọn Singapore (`ap-southeast-1`).
* **Công cụ cá nhân:** Đã cài đặt AWS CLI, Git và SSH Client trên máy máy tính làm việc.

---

### 2. Thiết lập Hạ tầng Mạng (Amazon VPC Multi-AZ)

Hạ tầng mạng Shopsflow được chia thành 6 subnets phân bổ trên 2 Availability Zones (`ap-southeast-1a` và `ap-southeast-1b`) để đảm bảo tính sẵn sàng cao và bảo mật tuyệt đối.

#### Bước 1: Khởi tạo VPC
1. Truy cập **AWS Console** → **VPC** → **Your VPCs** → **Create VPC**.
2. Thiết lập:
   * **VPC settings:** Chọn **VPC only**.
   * **Name tag:** `shopsflow-vpc`
   * **IPv4 CIDR block:** `10.0.0.0/16`
3. Click **Create VPC**.

![Bước 1: Khởi tạo Amazon VPC](/images/5-Workshop/1.jpg)

#### Bước 2: Tạo các Subnets
Truy cập **VPC** → **Subnets** → **Create subnet**. Chọn VPC `shopsflow-vpc`. Tạo lần lượt 6 subnets:

| Tên Subnet | CIDR Block | Availability Zone | Phân vùng |
|---|---|---|---|
| `shopsflow-public-1` | `10.0.1.0/24` | `ap-southeast-1a` | Public (ALB, NAT GW) |
| `shopsflow-public-2` | `10.0.2.0/24` | `ap-southeast-1b` | Public (ALB) |
| `shopsflow-private-app-1` | `10.0.3.0/24` | `ap-southeast-1a` | Private App (EC2 Backend) |
| `shopsflow-private-app-2` | `10.0.4.0/24` | `ap-southeast-1b` | Private App (EC2 Backend) |
| `shopsflow-private-db-1` | `10.0.5.0/24` | `ap-southeast-1a` | Private DB (RDS Primary) |
| `shopsflow-private-db-2` | `10.0.6.0/24` | `ap-southeast-1b` | Private DB (RDS Standby) |

![Bước 2: Tạo danh sách các Subnets](/images/5-Workshop/2.jpg)

#### Bước 3: Tạo Internet Gateway (IGW) & NAT Gateway
1. **Tạo Internet Gateway:**
   * Truy cập **Internet gateways** → **Create internet gateway**.
   * Name: `shopsflow-igw`. Click **Create**.
   * Actions → **Attach to VPC** → Chọn `shopsflow-vpc`.

![Bước 3: Tạo và Gán Internet Gateway](/images/5-Workshop/3.jpg)
2. **Tạo NAT Gateway:**
   * Truy cập **NAT gateways** → **Create NAT gateway**.
   * Name: `shopsflow-nat-gw`.
   * **Subnet:** Chọn `shopsflow-public-1` (Phải nằm trong Public Subnet).
   * **Elastic IP allocation ID:** Click **Allocate Elastic IP** cấp IP tĩnh cho NAT.
   * Click **Create NAT gateway** (đợi chuyển trạng thái *Available*).

![Bước 3.2: Tạo NAT Gateway với Elastic IP](/images/5-Workshop/4.jpg)

#### Bước 4: Thiết lập Bảng định tuyến (Route Tables)
1. **Public Route Table (`shopsflow-public-rt`):**
   * Target: `0.0.0.0/0` → `shopsflow-igw`.
   * Association: `shopsflow-public-1`, `shopsflow-public-2`.
2. **Private Route Table (`shopsflow-private-rt`):**
   * Target: `0.0.0.0/0` → `shopsflow-nat-gw`.
   * Association: `shopsflow-private-app-1`, `shopsflow-private-app-2`.
3. **DB Route Table (`shopsflow-db-rt`):**
   * Không cấu hình route ra ngoài Internet (cô lập hoàn toàn).
   * Association: `shopsflow-private-db-1`, `shopsflow-private-db-2`.

![Bước 4: Thiết lập Bảng định tuyến Route Tables](/images/5-Workshop/5.jpg)

---

### 3. Cấu hình Bảo mật (KMS & Secrets Manager)

#### Bước 1: Tạo AWS KMS Key
1. Truy cập **AWS KMS** → **Customer managed keys** → **Create key**.
2. Key type: **Symmetric**, Key usage: **Encrypt and decrypt**.
3. Alias: `shopsflow-kms-key`. Click **Create key**.

#### Bước 2: Tạo Secret trên Secrets Manager
1. Truy cập **Secrets Manager** → **Store a new secret**.
2. Secret type: **Other type of secret**.
3. Thêm các key/value nhạy cảm:
   * `SPRING_DATASOURCE_PASSWORD`: `ShopsflowPass123!`
   * `JWT_SECRET`: `ChuoiMatKhauJWTSecretMatGiaiMa64KyTuRandoom1234567890!`
   * `VNPAY_HASH_SECRET`: `VNPayHashSecretSandboxKey123!`
4. **Encryption key:** Chọn `shopsflow-kms-key`.
5. Secret name: `shopsflow/production/secrets`. Click **Store**.

---

### 4. Thiết lập Security Groups (Phân tầng Firewall)

Tạo 3 Security Groups kiểm soát kết nối phân cấp:

1. **ALB Security Group (`shopsflow-alb-sg`):**
   * Inbound: `HTTP` (Port 80) & `HTTPS` (Port 443) từ `0.0.0.0/0`.
2. **EC2 Security Group (`shopsflow-ec2-sg`):**
   * Inbound 1: `TCP` (Port 8080) với Source là `shopsflow-alb-sg` (chỉ nhận request từ ALB).
   * Inbound 2: `SSH` (Port 22) từ IP làm việc của bạn (hoặc qua Session Manager).
3. **RDS Security Group (`shopsflow-rds-sg`):**
   * Inbound: `PostgreSQL` (Port 5432) với Source là `shopsflow-ec2-sg` (chỉ nhận truy vấn từ EC2).

![Bước 5: Cấu hình phân tầng Security Groups](/images/5-Workshop/6.jpg)

---

### 5. Tạo IAM Role cho EC2 Backend

1. Truy cập **IAM** → **Roles** → **Create role** → Use case: **EC2**.
2. Attach AWS Managed Policies:
   * `CloudWatchAgentServerPolicy`
   * `AmazonS3FullAccess`
3. Thêm Inline Policy cấp quyền đọc Secret và giải mã KMS:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "secretsmanager:GetSecretValue",
           "kms:Decrypt"
         ],
         "Resource": "*"
       }
     ]
   }
   ```
4. Role Name: `ShopsflowEC2Role` → Click **Create role**.

![Bước 6: Khởi tạo IAM Role cho EC2 Backend](/images/5-Workshop/7.jpg)
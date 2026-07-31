---
title: "Prerequisites & Environment Setup"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

This section covers setting up the **Amazon VPC Multi-AZ** network infrastructure, creating an **AWS KMS** key, storing sensitive parameters in **AWS Secrets Manager**, configuring multi-tier **Security Groups**, and provisioning the **IAM Role**.

---

### 1. Prerequisites

* **AWS Account:** Account with Administrator Access.
* **AWS Region:** Singapore (`ap-southeast-1`).
* **Local Tools:** AWS CLI, Git, and an SSH Client installed on your workstation.

---

### 2. Network Infrastructure (Amazon VPC Multi-AZ)

The Shopsflow network is partitioned into 6 subnets distributed across 2 Availability Zones (`ap-southeast-1a` and `ap-southeast-1b`) for high availability and security isolation.

#### Step 1: Create VPC
1. Navigate to **AWS Console** → **VPC** → **Your VPCs** → **Create VPC**.
2. Configure:
   * **VPC settings:** Select **VPC only**.
   * **Name tag:** `shopsflow-vpc`
   * **IPv4 CIDR block:** `10.0.0.0/16`
3. Click **Create VPC**.

![Step 1: Amazon VPC Creation](/images/5-Workshop/1.jpg)

#### Step 2: Create Subnets
Navigate to **VPC** → **Subnets** → **Create subnet**. Select `shopsflow-vpc`. Create 6 subnets:

| Subnet Name | CIDR Block | Availability Zone | Tier |
|---|---|---|---|
| `shopsflow-public-1` | `10.0.1.0/24` | `ap-southeast-1a` | Public (ALB, NAT GW) |
| `shopsflow-public-2` | `10.0.2.0/24` | `ap-southeast-1b` | Public (ALB) |
| `shopsflow-private-app-1` | `10.0.3.0/24` | `ap-southeast-1a` | Private App (EC2 Backend) |
| `shopsflow-private-app-2` | `10.0.4.0/24` | `ap-southeast-1b` | Private App (EC2 Backend) |
| `shopsflow-private-db-1` | `10.0.5.0/24` | `ap-southeast-1a` | Private DB (RDS Primary) |
| `shopsflow-private-db-2` | `10.0.6.0/24` | `ap-southeast-1b` | Private DB (RDS Standby) |

![Step 2: Subnet List Configuration](/images/5-Workshop/2.jpg)

#### Step 3: Create Internet Gateway (IGW) & NAT Gateway
1. **Create Internet Gateway:**
   * Navigate to **Internet gateways** → **Create internet gateway**.
   * Name: `shopsflow-igw`. Click **Create**.
   * Actions → **Attach to VPC** → Select `shopsflow-vpc`.

![Step 3: Internet Gateway Creation and Attachment](/images/5-Workshop/3.jpg)
2. **Create NAT Gateway:**
   * Navigate to **NAT gateways** → **Create NAT gateway**.
   * Name: `shopsflow-nat-gw`.
   * **Subnet:** Select `shopsflow-public-1` (Must reside in a Public Subnet).
   * **Elastic IP allocation ID:** Click **Allocate Elastic IP** to assign a static public IP for the NAT gateway.
   * Click **Create NAT gateway** and wait for the status to transition to *Available*.

![Step 3.2: NAT Gateway Creation with Elastic IP](/images/5-Workshop/4.jpg)

#### Step 4: Configure Route Tables
1. **Public Route Table (`shopsflow-public-rt`):**
   * Target: `0.0.0.0/0` → `shopsflow-igw`.
   * Subnet Associations: `shopsflow-public-1`, `shopsflow-public-2`.
2. **Private Route Table (`shopsflow-private-rt`):**
   * Target: `0.0.0.0/0` → `shopsflow-nat-gw`.
   * Subnet Associations: `shopsflow-private-app-1`, `shopsflow-private-app-2`.
3. **DB Route Table (`shopsflow-db-rt`):**
   * No route to the Internet (fully isolated).
   * Associations: `shopsflow-private-db-1`, `shopsflow-private-db-2`.

![Step 4: Route Table Configuration](/images/5-Workshop/5.jpg)

---

### 3. Security Configuration (KMS & Secrets Manager)

#### Step 1: Create AWS KMS Key
1. Navigate to **AWS KMS** → **Customer managed keys** → **Create key**.
2. Key type: **Symmetric**, Key usage: **Encrypt and decrypt**.
3. Alias: `shopsflow-kms-key`. Click **Create key**.

#### Step 2: Store Secrets in Secrets Manager
1. Navigate to **Secrets Manager** → **Store a new secret**.
2. Secret type: **Other type of secret**.
3. Add sensitive key-value pairs:
   * `SPRING_DATASOURCE_PASSWORD`: `ShopsflowPass123!`
   * `JWT_SECRET`: `ChuoiMatKhauJWTSecretMatGiaiMa64KyTuRandoom1234567890!`
   * `VNPAY_HASH_SECRET`: `VNPayHashSecretSandboxKey123!`
4. **Encryption key:** Select `shopsflow-kms-key`.
5. Secret name: `shopsflow/production/secrets`. Click **Store**.

---

### 4. Security Groups (Multi-Tier Firewall)

Create 3 Security Groups for chained traffic control:

1. **ALB Security Group (`shopsflow-alb-sg`):**
   * Inbound: `HTTP` (Port 80) & `HTTPS` (Port 443) from `0.0.0.0/0`.
2. **EC2 Security Group (`shopsflow-ec2-sg`):**
   * Inbound 1: `TCP` (Port 8080) with Source `shopsflow-alb-sg` (traffic allowed only from ALB).
   * Inbound 2: `SSH` (Port 22) from your IP or Session Manager.
3. **RDS Security Group (`shopsflow-rds-sg`):**
   * Inbound: `PostgreSQL` (Port 5432) from `shopsflow-ec2-sg` (accept queries from EC2 only).

![Step 5: Security Groups Tiering Setup](/images/5-Workshop/6.jpg)

---

### 5. Create IAM Role for EC2 Backend

1. Navigate to **IAM** → **Roles** → **Create role** → Service: **EC2**.
2. Attach AWS Managed Policies:
   * `CloudWatchAgentServerPolicy`
   * `AmazonS3FullAccess`
3. Add an Inline Policy for Secrets Manager and KMS decryption:
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
4. Role name: `ShopsflowEC2Role`. Click **Create role**.

![Step 6: IAM Role Setup for EC2 Backend](/images/5-Workshop/7.jpg)
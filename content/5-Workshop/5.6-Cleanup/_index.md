---
title: "Clean Up Resources"
date: 2026-06-15
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Enterprise Highly Available (HA) architectures utilize AWS services billed on an hourly basis (such as NAT Gateway and Application Load Balancer). Ensure that you perform resource cleanup in the exact order below to prevent unwanted charges on your AWS account.

> ⚠️ **Important:** Perform resource cleanup in the exact sequence listed below to avoid dependency errors.

---

### Step-by-Step Resource Cleanup

#### 1. Delete CloudFront Distribution & WAF Web ACL
1. Navigate to **AWS Console** → **CloudFront** → Select the `shopsflow` distribution → Click **Disable**.
2. Wait for status to change to *Disabled* (~5 minutes) → Select distribution → Click **Delete**.
3. Navigate to **AWS WAF & Shield** → **Web ACLs** → Disassociate CloudFront → Click **Delete** for Web ACL `shopsflow-waf`.

#### 2. Delete Application Load Balancer (ALB) & Target Group
1. Navigate to **EC2** → **Load Balancers** → Select `shopsflow-alb` → Click **Actions** → **Delete load balancer**.
2. Navigate to **Target Groups** → Select `shopsflow-tg` → Click **Actions** → **Delete**.

#### 3. Delete Auto Scaling Group (ASG) & Launch Template
1. Navigate to **EC2** → **Auto Scaling Groups** → Select `shopsflow-asg` → Click **Delete** (type `delete` to confirm).
   * *Note:* Deleting the ASG automatically terminates all EC2 instances running inside Private Subnets.
2. Navigate to **Launch Templates** → Select `shopsflow-lt` → Click **Actions** → **Delete template**.

#### 4. Delete RDS PostgreSQL Multi-AZ Database
1. Navigate to **RDS** → **Databases** → Select `shopsflow-postgres` → Click **Actions** → **Delete**.
2. Uncheck *Create final snapshot?*, type `delete me` to confirm deletion.
3. After the database is deleted, navigate to **Subnet groups** → Delete `shopsflow-db-subnet-group`.

#### 5. Delete S3 Buckets
1. Navigate to **S3** → Select, empty (**Empty**), and delete (**Delete**) both buckets:
   * Frontend bucket: `shopsflow-frontend-<your-account-id>`
   * Database backup bucket: `shopsflow-db-backups-<your-account-id>`

#### 6. Delete Secrets Manager & KMS Key
1. Navigate to **Secrets Manager** → Select secret `shopsflow/production/secrets` → Actions → **Delete secret** (7-day recovery window).
2. Navigate to **KMS** → Select key `shopsflow-kms-key` → Actions → **Schedule key deletion** (7-day window).

#### 7. Release Network Resources (NAT Gateway & VPC)
```bash
# 1. Delete NAT Gateway (~2 minutes)
aws ec2 delete-nat-gateway --nat-gateway-id <NAT_GATEWAY_ID>

# 2. Release Elastic IP
aws ec2 release-address --allocation-id <ELASTIC_IP_ALLOC_ID>

# 3. Delete S3 VPC Endpoint
aws ec2 delete-vpc-endpoints --vpc-endpoint-ids <ENDPOINT_ID>

# 4. Detach and delete Internet Gateway
aws ec2 detach-internet-gateway --internet-gateway-id <IGW_ID> --vpc-id <VPC_ID>
aws ec2 delete-internet-gateway --internet-gateway-id <IGW_ID>

# 5. Delete Subnets, Route Tables, Security Groups, and VPC
aws ec2 delete-vpc --vpc-id <VPC_ID>
```

#### 8. Delete IAM Role & CloudWatch Resources
1. Delete IAM Role `ShopsflowEC2Role`.
2. Delete CloudWatch Dashboard `ShopsflowDashboard`, Alarms, and Log Group `/shopsflow/application`.

---

### Reflection & Key Takeaways

#### 1. Challenges Encountered
* **Multi-AZ Network Complexity:** Partitioning the VPC into 6 subnets across 2 Availability Zones required precise Route Table configurations. Initially, EC2 instances in Private Subnets failed to reach the Internet to install Docker due to a misconfigured route to the NAT Gateway.
* **Secrets Manager & KMS Integration:** Configuring User Data for EC2 to fetch database credentials and decrypt the JWT secret caused access errors until the IAM Role was granted explicit `kms:Decrypt` permissions.
* **CloudFront Routing & CORS:** Routing both Frontend S3 and Backend ALB under a single CloudFront distribution with `/api/*` required configuring Cache Policy and Origin Request Policy to prevent CORS issues.

#### 2. Lessons Learned
* **Deep Understanding of High Availability (HA):** Mastered practical concepts of ALB and ASG in load distribution, health checks, and self-healing.
* **Defense in Depth Security:** Understood the necessity of completely isolating databases and backend applications inside Private Subnets behind controlled entry points (WAF, CloudFront, ALB).
* **Cost Optimization with VPC Endpoints:** Learned how Gateway VPC Endpoints transfer large database backups to S3 over AWS internal networks without traversing NAT Gateways, preventing costly bandwidth processing fees.

#### 3. Future Enhancements
* **Infrastructure as Code (IaC):** Utilize **Terraform** modules to define all AWS infrastructure as code for automated, repeatable deployments.
* **Advanced Serverless/Containerization:** Migrate Spring Boot Backend onto **AWS ECS Fargate** to eliminate EC2 server management altogether.
---
title: "Deploy Spring Boot Backend API"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 5.3.2 </b> "
---

In this step, you will create the **Application Load Balancer (ALB)**, an **EC2 Launch Template** that auto-starts the Shopsflow Spring Boot API inside Docker, and an **Auto Scaling Group (ASG)** to provide high availability across two private subnets.

---

### 1. Create ALB Target Group

1. Navigate to **EC2** → **Target Groups** → **Create target group**.

| Field                 | Value              |
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
| Timeout               | `5 seconds`        |
| Interval              | `30 seconds`       |

2. Click **Next** → **Create target group** (no instances registered yet).

---

### 2. Create Application Load Balancer

1. Navigate to **EC2** → **Load Balancers** → **Create load balancer** → **Application Load Balancer**.

| Field           | Value                                                    |
| --------------- | -------------------------------------------------------- |
| Name            | `shopsflow-alb`                                          |
| Scheme          | Internet-facing                                          |
| IP address type | IPv4                                                     |
| VPC             | `shopsflow-vpc`                                          |
| Subnets         | `shopsflow-public-1` (AZ-a), `shopsflow-public-2` (AZ-b) |
| Security groups | `shopsflow-alb-sg`                                       |

2. Under **Listeners**:
   - Add listener: Protocol `HTTP`, Port `80`
   - Default action: Forward to `shopsflow-tg`

3. Click **Create load balancer**.

{{% notice note %}}
Copy the **DNS name** of the ALB (e.g., `shopsflow-alb-123456.ap-southeast-1.elb.amazonaws.com`) — you will need it when building the frontend in Section 5.4.
{{% /notice %}}

---

### 3. Prepare the EC2 User Data Script

The EC2 instance will automatically pull credentials from **Secrets Manager** and start the Shopsflow backend container on boot.

Replace `<RDS_ENDPOINT>` with the endpoint you copied in step 5.3.1.

```bash
#!/bin/bash
yum update -y
yum install -y docker aws-cli postgresql15
systemctl start docker
systemctl enable docker
usermod -aG docker ec2-user

# Wait for network stabilization
sleep 10

# Fetch all secrets from Secrets Manager
SECRETS=$(aws secretsmanager get-secret-value \
  --secret-id shopsflow/production/secrets \
  --region ap-southeast-1 \
  --query SecretString --output text)

export DB_URL=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['SPRING_DATASOURCE_URL'])")
export DB_USER=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['SPRING_DATASOURCE_USERNAME'])")
export DB_PASS=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['SPRING_DATASOURCE_PASSWORD'])")
export JWT_SECRET=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['JWT_SECRET'])")
export VNPAY_HASH=$(echo $SECRETS | python3 -c "import sys,json; print(json.load(sys.stdin)['VNPAY_HASH_SECRET'])")

# Start Shopsflow Backend container
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

### 4. Create EC2 Launch Template

1. Navigate to **EC2** → **Launch Templates** → **Create launch template**.

| Field                | Value                      |
| -------------------- | -------------------------- |
| Name                 | `shopsflow-lt`             |
| AMI                  | Amazon Linux 2023 (latest) |
| Instance type        | `t3.small`                 |
| Key pair             | Your existing SSH key pair |
| Security groups      | `shopsflow-ec2-sg`         |
| IAM instance profile | `ShopsflowEC2Role`         |

2. Under **Advanced details** → **User data**: paste the script from Step 3.
3. Click **Create launch template**.

---

### 5. Create Auto Scaling Group

1. Navigate to **EC2** → **Auto Scaling Groups** → **Create Auto Scaling group**.

| Field           | Value                                                |
| --------------- | ---------------------------------------------------- |
| Name            | `shopsflow-asg`                                      |
| Launch template | `shopsflow-lt`                                       |
| VPC             | `shopsflow-vpc`                                      |
| Subnets         | `shopsflow-private-app-1`, `shopsflow-private-app-2` |

2. Under **Load balancing** → Attach to existing load balancer → Choose `shopsflow-tg`.
3. Under **Health checks** → Turn on ELB health checks.
4. Under **Group size**:

|                  | Value |
| ---------------- | ----- |
| Desired capacity | `2`   |
| Minimum capacity | `1`   |
| Maximum capacity | `4`   |

5. Under **Automatic scaling** → Add a **Target tracking scaling policy**:
   - Metric: Average CPU utilization
   - Target value: `60`

6. Click **Create Auto Scaling group**.

---

### 6. Verify Backend Health

After about **3–5 minutes**, the ASG will launch EC2 instances. Verify the backend is healthy:

```bash
# Test via ALB DNS
curl http://<ALB_DNS_NAME>/actuator/health
# Expected: {"status":"UP"}

# Check Flyway migration logs on an EC2 instance
aws ssm start-session --target <INSTANCE_ID>
docker logs shopsflow-backend 2>&1 | grep -i flyway
# Expected:
# Successfully applied 12 migrations to schema "public"
```

Navigate to **EC2** → **Target Groups** → `shopsflow-tg` → **Targets** tab and confirm all registered targets show status **Healthy**.

**The Spring Boot backend is now running in a highly available configuration across two Availability Zones, behind the Application Load Balancer.**

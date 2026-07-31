---
title: "Provision RDS PostgreSQL Database"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

In this step, you will provision an **Amazon RDS PostgreSQL** instance in **Multi-AZ** mode as the primary database for Shopsflow. The instance will reside in private DB subnets and will only be accessible from the EC2 backend tier via the `shopsflow-rds-sg` Security Group.

---

### 1. Create DB Subnet Group

Before creating the RDS instance, you must define which subnets it can reside in.

1. Navigate to **AWS Console** → **RDS** → **Subnet groups** → **Create DB subnet group**.
2. Fill in the details:

| Field | Value |
|---|---|
| Name | `shopsflow-db-subnet-group` |
| Description | Private DB subnets for Shopsflow RDS |
| VPC | `shopsflow-vpc` |
| Availability Zones | `ap-southeast-1a`, `ap-southeast-1b` |
| Subnets | `shopsflow-private-db-1`, `shopsflow-private-db-2` |

3. Click **Create**.

---

### 2. Create RDS PostgreSQL Instance

1. Navigate to **RDS** → **Databases** → **Create database**.
2. Choose **Standard create**.
3. Under **Engine options**: Select **PostgreSQL**, version **16.x**.
4. Under **Templates**: Select **Free tier** for testing or **Production** for real deployments.

5. Under **Settings**:

| Field | Value |
|---|---|
| DB instance identifier | `shopsflow-postgres` |
| Master username | `shopsflow_admin` |
| Master password | `ShopsflowPass123!` |

> ⚠️ You will store this password in **AWS Secrets Manager** in a later step — do not use this password directly in application code.

6. Under **Instance configuration**:
   * DB instance class: `db.t3.micro`

7. Under **Availability & durability**:
   * ✅ **Multi-AZ DB instance** — creates a synchronous standby in a second AZ for failover.

8. Under **Storage**:
   * Storage type: `gp3`
   * Allocated storage: `20 GiB`

9. Under **Connectivity**:

| Field | Value |
|---|---|
| VPC | `shopsflow-vpc` |
| DB Subnet Group | `shopsflow-db-subnet-group` |
| Public access | ❌ **No** |
| VPC security group | `shopsflow-rds-sg` |

10. Under **Additional configuration**:
    * Initial database name: `shopsflow`
    * ✅ Enable automated backups
    * Backup retention period: **7 days**

11. Click **Create database**.

{{% notice info %}}
RDS provisioning takes approximately **5–10 minutes**. The status will change from *Creating* → *Backing-up* → *Available*. Do not proceed to the next step until the status is **Available**.
{{% /notice %}}

---

### 3. Copy the RDS Endpoint

Once the database is available:

1. Navigate to **RDS** → **Databases** → Click `shopsflow-postgres`.
2. Under the **Connectivity & security** tab, copy the **Endpoint** value:

```
shopsflow-postgres.xxxxxxxx.ap-southeast-1.rds.amazonaws.com
```

Save this value — you will use it in the EC2 User Data script in the next step.

---

### 4. Store Credentials in Secrets Manager

1. Navigate to **AWS Console** → **Secrets Manager** → **Store a new secret**.
2. Choose **Other type of secret**.
3. Add key–value pairs:

| Key | Value |
|---|---|
| `SPRING_DATASOURCE_URL` | `jdbc:postgresql://<RDS_ENDPOINT>:5432/shopsflow` |
| `SPRING_DATASOURCE_USERNAME` | `shopsflow_admin` |
| `SPRING_DATASOURCE_PASSWORD` | `ShopsflowPass123!` |
| `JWT_SECRET` | *(generate a 64-char random string)* |
| `VNPAY_HASH_SECRET` | *(your VNPay Sandbox hash secret)* |

4. Under **Encryption key**: Select the **KMS Customer Managed Key** you created earlier (`shopsflow-kms-key`).
5. Secret name: `shopsflow/production/secrets`
6. Click **Store**.

✅ **Result:** All sensitive credentials are now stored encrypted. The EC2 backend will retrieve them at startup via the `aws secretsmanager get-secret-value` API, using the `ShopsflowEC2Role` IAM permissions.

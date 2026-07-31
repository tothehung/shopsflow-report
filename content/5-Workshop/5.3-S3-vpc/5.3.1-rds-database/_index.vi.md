---
title: "Khởi tạo RDS PostgreSQL"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 5.3.1 </b> "
---

Ở bước này, bạn sẽ khởi tạo **Amazon RDS PostgreSQL** ở chế độ **Multi-AZ** làm cơ sở dữ liệu chính cho Shopsflow. Instance sẽ nằm trong private DB subnets và chỉ có thể được truy cập từ tầng EC2 Backend qua Security Group `shopsflow-rds-sg`.

---

### 1. Tạo DB Subnet Group

Trước khi tạo RDS instance, bạn phải xác định các subnets mà nó có thể đặt vào.

1. Truy cập **AWS Console** → **RDS** → **Subnet groups** → **Create DB subnet group**.
2. Điền thông tin:

| Trường             | Giá trị                                            |
| ------------------ | -------------------------------------------------- |
| Name               | `shopsflow-db-subnet-group`                        |
| Description        | Private DB subnets for Shopsflow RDS               |
| VPC                | `shopsflow-vpc`                                    |
| Availability Zones | `ap-southeast-1a`, `ap-southeast-1b`               |
| Subnets            | `shopsflow-private-db-1`, `shopsflow-private-db-2` |

3. Click **Create**.

---

### 2. Tạo RDS PostgreSQL Instance

1. Truy cập **RDS** → **Databases** → **Create database**.
2. Chọn **Standard create**.
3. Tại mục **Engine options**: Chọn **PostgreSQL**, phiên bản **16.x**.
4. Tại mục **Templates**: Chọn **Free tier** để thực hành hoặc **Production** khi triển khai thực tế.

5. Tại mục **Settings**:

| Trường                 | Giá trị              |
| ---------------------- | -------------------- |
| DB instance identifier | `shopsflow-postgres` |
| Master username        | `shopsflow_admin`    |
| Master password        | `ShopsflowPass123!`  |

> Mật khẩu này sẽ được lưu vào **AWS Secrets Manager** ở bước sau — không đặt trực tiếp vào code ứng dụng.

6. Tại mục **Instance configuration**:
   - DB instance class: `db.t3.micro`

7. Tại mục **Availability & durability**:
   - **Multi-AZ DB instance** — tạo bản dự phòng đồng bộ ở AZ thứ hai để failover tự động.

8. Tại mục **Storage**:
   - Storage type: `gp3`
   - Allocated storage: `20 GiB`

9. Tại mục **Connectivity**:

| Trường             | Giá trị                     |
| ------------------ | --------------------------- |
| VPC                | `shopsflow-vpc`             |
| DB Subnet Group    | `shopsflow-db-subnet-group` |
| Public access      | **No**                      |
| VPC security group | `shopsflow-rds-sg`          |

10. Tại mục **Additional configuration**:
    - Initial database name: `shopsflow`
    - Bật automated backups
    - Backup retention period: **7 ngày**

11. Click **Create database**.

![RDS database-shopsflow được tạo thành công (Status: Available, PostgreSQL)](/images/5-Workshop/5.3-S3-vpc/rds-created.jpg)

{{% notice info %}}
Quá trình khởi tạo RDS mất khoảng **5–10 phút**. Trạng thái sẽ chuyển từ _Creating_ → _Backing-up_ → _Available_. Không tiếp tục bước tiếp theo cho đến khi trạng thái là **Available**.
{{% /notice %}}

---

### 3. Sao chép RDS Endpoint

Khi database đã Available:

1. Truy cập **RDS** → **Databases** → Click `shopsflow-postgres`.
2. Trong tab **Connectivity & security**, sao chép giá trị **Endpoint**:

```
shopsflow-postgres.xxxxxxxx.ap-southeast-1.rds.amazonaws.com
```

Lưu lại giá trị này — bạn sẽ dùng nó trong User Data script ở bước tiếp theo.

---

### 4. Lưu Credentials vào Secrets Manager

1. Truy cập **AWS Console** → **Secrets Manager** → **Store a new secret**.
2. Chọn **Other type of secret**.
3. Thêm các key–value:

| Key                          | Value                                             |
| ---------------------------- | ------------------------------------------------- |
| `SPRING_DATASOURCE_URL`      | `jdbc:postgresql://<RDS_ENDPOINT>:5432/shopsflow` |
| `SPRING_DATASOURCE_USERNAME` | `shopsflow_admin`                                 |
| `SPRING_DATASOURCE_PASSWORD` | `ShopsflowPass123!`                               |
| `JWT_SECRET`                 | _(tạo chuỗi ngẫu nhiên 64 ký tự)_                 |
| `VNPAY_HASH_SECRET`          | _(hash secret VNPay Sandbox của bạn)_             |

4. Tại mục **Encryption key**: Chọn **KMS Customer Managed Key** đã tạo trước đó (`shopsflow-kms-key`).
5. Secret name: `shopsflow/production/secrets`
6. Click **Store**.



**Kết quả:** Toàn bộ thông tin nhạy cảm đã được lưu mã hóa. EC2 Backend sẽ lấy chúng khi khởi động qua API `aws secretsmanager get-secret-value` với quyền IAM của `ShopsflowEC2Role`.

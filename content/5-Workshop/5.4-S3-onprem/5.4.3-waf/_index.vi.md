---
title: "Bảo vệ bằng AWS WAF"
date: 2026-06-15
weight: 3
chapter: false
pre: " <b> 5.4.3 </b> "
---

Ở bước này, bạn sẽ tạo **AWS WAF Web ACL** và gắn vào CloudFront distribution. WAF sẽ kiểm tra mọi request HTTP/HTTPS đến Shopsflow Storefront và chặn các mối đe dọa web phổ biến trước khi chúng chạm đến ứng dụng.

---

### 1. Tại sao cần WAF cho Shopsflow?

Shopsflow Storefront là ứng dụng React public-facing tiếp xúc với API backend. Nếu không có WAF:
- Kẻ tấn công có thể thăm dò API tìm lỗ hổng **SQL Injection** trong các tham số tìm kiếm và lọc.
- Bot có thể **duyệt brute-force Product ID** hoặc tấn công **credential stuffing** vào endpoint đăng nhập.
- Input độc hại có thể **vượt qua validation** phía frontend.

AWS WAF với Managed Rule Groups cung cấp bảo vệ tức thì mà không cần viết custom rules.

---

### 2. Tạo WAF Web ACL

{{% notice warning %}}
AWS WAF cho CloudFront **bắt buộc phải tạo ở region US East (N. Virginia) `us-east-1`**, bất kể các tài nguyên khác của bạn đang ở region nào.
{{% /notice %}}

1. Truy cập **AWS Console** → Chuyển region sang **US East (N. Virginia)**.
2. Truy cập **WAF & Shield** → **Web ACLs** → **Create web ACL**.

#### Cài đặt chung

| Trường | Giá trị |
|---|---|
| Name | `shopsflow-waf` |
| Resource type | Amazon CloudFront distributions |
| Region | Global (CloudFront) |

---

### 3. Thêm Managed Rule Groups

Click **Add rules** → **Add managed rule groups** → Bật 3 rule group sau:

#### Rule Group 1: Common Web Exploits
- **Tên:** `AWSManagedRulesCommonRuleSet`
- **Mục đích:** Chặn các lỗ hổng web phổ biến (XSS, path traversal, HTTP flooding)
- **Action:** Block

#### Rule Group 2: Known Bad Inputs
- **Tên:** `AWSManagedRulesKnownBadInputsRuleSet`
- **Mục đích:** Chặn các request khớp với pattern exploit đã biết, như Log4SHELL
- **Action:** Block

#### Rule Group 3: SQL Injection Protection

- **Name:** `AWSManagedRulesSQLiRuleSet`
- **Purpose:** Blocks SQL injection attacks in query strings, body, headers, and URI paths
- **Action:** Block

![Bước 13: Cấu hình các bộ Managed Rule Groups trên AWS WAF Web ACL](/images/5-Workshop/14.jpg)

Click **Add rules**.

---

### 4. Thiết lập Default Action và Priority

1. Tại mục **Set rule priority**: Giữ thứ tự mặc định (rules được đánh giá từ trên xuống dưới).
2. Tại mục **Default action**: **Allow** — WAF cho phép toàn bộ traffic không khớp với các rules trên.

---

### 5. Cấu hình CloudWatch Metrics

Tại mục **Configure metrics**:
* Web ACL metric name: `shopsflow-waf-metrics`
* Bật sampled requests — cho phép xem các request mẫu trong WAF console để debug.

Click **Next** → **Next** → **Create web ACL**.

---

### 6. Gắn WAF vào CloudFront

1. Truy cập **WAF** → **Web ACLs** → Chọn `shopsflow-waf`.
2. Click tab **Associated AWS resources** → **Add AWS resources**.
3. Chọn **Amazon CloudFront distribution** → Chọn distribution `shopsflow`.
4. Click **Add**.

{{% notice note %}}
Mất **1–2 phút** để WAF association lan truyền toàn cầu trên tất cả CloudFront edge locations.
{{% /notice %}}

---

### 7. Kiểm tra WAF bảo vệ

#### Test 1: Chặn SQL Injection
```bash
# Mô phỏng SQL Injection trong query parameter
curl -I "https://dxxxxx.cloudfront.net/api/products?search=1'+OR+'1'='1"
# Kết quả mong đợi: HTTP/2 403 (bị WAF chặn - SQLi rule)
```

#### Test 2: Chặn XSS
```bash
# Mô phỏng payload XSS
curl -I "https://dxxxxx.cloudfront.net/?q=<script>alert(1)</script>"
# Kết quả mong đợi: HTTP/2 403 (bị WAF chặn - CommonRuleSet)
```

#### Test 3: Request bình thường được phép qua
```bash
# Request duyệt sản phẩm bình thường phải thành công
curl -I "https://dxxxxx.cloudfront.net/api/products?page=1&size=10"
# Kết quả mong đợi: HTTP/2 200 OK
```

![Kiểm tra AWS WAF Chặn SQLi & XSS](/images/5-Workshop/5.4-S3-onprem/result.png)

---

### 8. Xem WAF Metrics trên CloudWatch

1. Truy cập **CloudWatch** → **Metrics** → **WAF** → `shopsflow-waf-metrics`.
2. Theo dõi:
   * `BlockedRequests` — số request bị chặn theo từng rule
   * `AllowedRequests` — tổng traffic hợp lệ
   * `CountedRequests` — request khớp với count-only rules

**Frontend Shopsflow đã được bảo vệ bởi AWS WAF. Toàn bộ SQL Injection, XSS và các input độc hại đã biết đều bị tự động chặn tại CloudFront edge trước khi chạm đến ALB và Backend.**

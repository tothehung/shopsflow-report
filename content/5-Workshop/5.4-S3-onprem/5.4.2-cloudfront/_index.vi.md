---
title: "Tạo CloudFront Distribution"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 5.4.2 </b> "
---

Ở bước này, bạn sẽ tạo **Amazon CloudFront** distribution lấy nội dung từ S3 bucket qua **Origin Access Control (OAC)** để phục vụ Frontend tĩnh, đồng thời cấu hình định tuyến `/api/*` về **Application Load Balancer (ALB)** để gộp chung domain và loại bỏ lỗi CORS.

---

### 1. Tạo CloudFront Distribution

1. Truy cập **AWS Console** → **CloudFront** → **Create distribution**.

![Kiến trúc CloudFront CDN & S3 OAC](/images/5-Workshop/5.4-S3-onprem/diagram3.png)

#### Cấu hình Origin (S3 Frontend)

| Trường | Giá trị |
|---|---|
| Origin domain | `shopsflow-frontend-<your-account-id>.s3.ap-southeast-1.amazonaws.com` |
| Origin access | **Origin access control settings (recommended)** |

2. Click **Create new OAC**:

| Trường | Giá trị |
|---|---|
| Name | `shopsflow-oac` |
| Signing behavior | Sign requests (recommended) |

![CloudFront distribution fe-cloudfront được tạo thành công với OAC](/images/5-Workshop/5.4-S3-onprem/cloudfront-distribution.jpg)

3. Click **Create**.

#### Cấu hình Default Cache Behavior

| Trường | Giá trị |
|---|---|
| Viewer protocol policy | Redirect HTTP to HTTPS |
| Allowed HTTP methods | GET, HEAD |
| Cache policy | `CachingOptimized` (AWS Managed) |
| Origin request policy | `CORS-S3Origin` |

#### Cấu hình Settings

| Trường | Giá trị |
|---|---|
| Default root object | `index.html` |
| Price class | Use all edge locations |

4. Click **Create distribution**.

---

### 2. Cập nhật S3 Bucket Policy cho OAC

1. Copy policy từ banner nhắc của CloudFront.
2. Truy cập **S3** → `shopsflow-frontend-<your-account-id>` → **Permissions** → **Bucket policy** → **Edit**.
3. Dán policy:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::shopsflow-frontend-<your-account-id>/*",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceArn": "arn:aws:cloudfront::<account-id>:distribution/<distribution-id>"
                }
            }
        }
    ]
}
```

4. Click **Save changes**.

---

### 3. Cấu hình Custom Error Responses (SPA Routing)

React SPA sử dụng client-side routing. Cấu hình CloudFront chuyển hướng lỗi `403` và `404` về `/index.html` với mã HTTP `200`:

1. Truy cập CloudFront distribution → tab **Error pages** → **Create custom error response**.
2. **HTTP Error 403:** Response page path `/index.html`, HTTP Response code `200`.
3. **HTTP Error 404:** Response page path `/index.html`, HTTP Response code `200`.

---

### 4. Cấu hình Origin & Behavior cho Backend API (`/api/*`)

Để gộp chung Frontend S3 và Backend ALB trên cùng tên miền CloudFront, loại bỏ triệt để rủi ro CORS:

1. Tại tab **Origins** → Click **Create origin**:
   * **Origin domain:** Chọn DNS Name của ALB (`shopsflow-alb-xxxx.ap-southeast-1.elb.amazonaws.com`).
   * **Protocol:** HTTP Only.
   * Click **Create origin**.
2. Tại tab **Behaviors** → Click **Create behavior**:
   * **Path pattern:** `/api/*`
   * **Target origin:** Chọn ALB Origin vừa tạo.
   * **Allowed HTTP methods:** `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE`.
   * **Cache policy:** `CachingDisabled` (dữ liệu API động không lưu cache).
   * **Origin request policy:** `AllViewerAndCloudFrontHeaders-2022-06`.
   * Click **Create behavior**.

---

### 5. Kiểm tra Distribution

Mở trình duyệt và truy cập:
```
https://dxxxxx.cloudfront.net
```

**Kết quả mong đợi:** Shopsflow Storefront tải thành công qua HTTPS, các API call `/api/*` được CloudFront tự động chuyển hướng về ALB Backend xử lý mượt mà không gặp lỗi CORS.

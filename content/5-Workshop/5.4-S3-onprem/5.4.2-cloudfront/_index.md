---
title: "Create CloudFront Distribution"
date: 2026-06-15
weight: 2
chapter: false
pre: " <b> 5.4.2 </b> "
---

In this step, you will create an **Amazon CloudFront** distribution to serve static frontend content from S3 via **Origin Access Control (OAC)**, and configure a path pattern behavior for `/api/*` pointing to the **Application Load Balancer (ALB)** to eliminate CORS issues.

---

### 1. Create CloudFront Distribution

1. Navigate to **AWS Console** → **CloudFront** → **Create distribution**.

![CloudFront CDN & S3 OAC Architecture](/images/5-Workshop/5.4-S3-onprem/diagram3.png)

#### Origin Settings (S3 Frontend)

| Field | Value |
|---|---|
| Origin domain | `shopsflow-frontend-<your-account-id>.s3.ap-southeast-1.amazonaws.com` |
| Origin access | **Origin access control settings (recommended)** |

2. Click **Create new OAC**:

| Field | Value |
|---|---|
| Name | `shopsflow-oac` |
| Signing behavior | Sign requests (recommended) |

![Step 12: CloudFront Distribution & Origin Access Control (OAC) Setup](/images/5-Workshop/13.jpg)

3. Click **Create**.

#### Default Cache Behavior Settings

| Field | Value |
|---|---|
| Viewer protocol policy | Redirect HTTP to HTTPS |
| Allowed HTTP methods | GET, HEAD |
| Cache policy | `CachingOptimized` (AWS Managed) |
| Origin request policy | `CORS-S3Origin` |

#### Settings

| Field | Value |
|---|---|
| Default root object | `index.html` |
| Price class | Use all edge locations |

4. Click **Create distribution**.

---

### 2. Update S3 Bucket Policy for OAC

1. Copy the bucket policy from the banner.
2. Navigate to **S3** → `shopsflow-frontend-<your-account-id>` → **Permissions** → **Bucket policy** → **Edit**.
3. Paste the policy:

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

### 3. Configure Custom Error Responses (SPA Routing)

React Router uses client-side routing. Configure CloudFront to redirect `403` and `404` errors to `/index.html` with HTTP `200`:

1. Navigate to your CloudFront distribution → **Error pages** → **Create custom error response**.
2. **HTTP Error 403:** Response page path `/index.html`, HTTP Response code `200`.
3. **HTTP Error 404:** Response page path `/index.html`, HTTP Response code `200`.

---

### 4. Configure Backend API Origin & Behavior (`/api/*`)

To serve both Frontend S3 assets and Backend API requests under a single CloudFront domain, eliminating CORS challenges:

1. Under the **Origins** tab → Click **Create origin**:
   * **Origin domain:** Select ALB DNS Name (`shopsflow-alb-xxxx.ap-southeast-1.elb.amazonaws.com`).
   * **Protocol:** HTTP Only.
   * Click **Create origin**.
2. Under the **Behaviors** tab → Click **Create behavior**:
   * **Path pattern:** `/api/*`
   * **Target origin:** Select ALB Origin created above.
   * **Allowed HTTP methods:** `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE`.
   * **Cache policy:** `CachingDisabled` (dynamic API data should not be cached).
   * **Origin request policy:** `AllViewerAndCloudFrontHeaders-2022-06`.
   * Click **Create behavior**.

---

### 5. Verify Distribution

Open your browser and navigate to:
```
https://dxxxxx.cloudfront.net
```

**Expected:** The Shopsflow Storefront loads over HTTPS, and API calls `/api/*` are seamlessly forwarded by CloudFront to the ALB Backend without CORS errors.

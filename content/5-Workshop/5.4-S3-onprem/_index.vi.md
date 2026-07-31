---
title: "Triển khai Frontend (S3 + CloudFront + WAF)"
date: 2026-06-15
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

Phần này hướng dẫn build và deploy **React + Vite** Frontend lên **Amazon S3** để lưu trữ tĩnh, phân phối toàn cầu qua **Amazon CloudFront CDN**, và bảo vệ bằng **AWS WAF**.

### Tổng quan kiến trúc

```
Trình duyệt
    │  HTTPS
    ▼
[AWS WAF]  ──── Chặn: SQLi, XSS, Bad Inputs
    │
    ▼
[CloudFront CDN]  ──── Global edge locations  ──── Chỉ HTTPS
    │  Origin Access Control (OAC)
    ▼
[S3 Bucket]  ──── Private (không có public access)
    shopsflow-frontend-<account-id>/
    ├── index.html
    └── assets/ (JS, CSS bundles)
```

**Các quyết định thiết kế quan trọng:**
- S3 bucket **không có public access** — CloudFront đọc qua OAC signed requests.
- **SPA error pages**: 403/404 → `index.html` với HTTP 200 để hỗ trợ React Router.
- **WAF** được gắn ở cấp CloudFront, kiểm tra mọi request tại edge.

#### Nội dung

1. [Build & Upload Frontend lên S3](5.4.1-s3-frontend/)
2. [Tạo CloudFront Distribution](5.4.2-cloudfront/)
3. [Bảo vệ bằng AWS WAF](5.4.3-waf/)

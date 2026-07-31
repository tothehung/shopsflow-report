---
title: "Deploy Frontend (S3 + CloudFront + WAF)"
date: 2026-06-15
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

This section guides you through building and deploying the **React + Vite** frontend onto **Amazon S3** for static hosting, distributing it globally via **Amazon CloudFront CDN**, and protecting it with **AWS WAF**.

### Architecture Overview

```
User Browser
    │  HTTPS
    ▼
[AWS WAF]  ──── Blocks: SQLi, XSS, Bad Inputs
    │
    ▼
[CloudFront CDN]  ──── Global edge locations  ──── HTTPS only
    │  Origin Access Control (OAC)
    ▼
[S3 Bucket]  ──── Private (no public access)
    shopsflow-frontend-<account-id>/
    ├── index.html
    └── assets/ (JS, CSS bundles)
```

**Key design decisions:**
- S3 bucket has **no public access** — CloudFront reads it via OAC signed requests.
- **SPA error pages**: 403/404 → `index.html` with HTTP 200 to support React Router.
- **WAF** is attached at the CloudFront level, inspecting all requests at the edge.

#### Content

1. [Build & Upload Frontend to S3](5.4.1-s3-frontend/)
2. [Create CloudFront Distribution](5.4.2-cloudfront/)
3. [Protect with AWS WAF](5.4.3-waf/)

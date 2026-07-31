---
title: "Build & Upload Frontend to S3"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 5.4.1 </b> "
---

In this step, you will build the **React + Vite** production bundle and upload the static assets to an **Amazon S3** bucket. S3 will serve as the origin for the CloudFront CDN distribution.

---

### 1. Build the Production Bundle

On your local machine, in the `frontend/` directory of the Shopsflow project:

```bash
cd shopsflow/frontend

# Install all dependencies
npm install

# Set the ALB DNS as the API backend URL
# Replace <ALB_DNS_NAME> with the value from Section 5.3
echo "VITE_API_BASE_URL=http://<ALB_DNS_NAME>" > .env.production
echo "VITE_APP_ENV=production" >> .env.production

# Build the production bundle
npm run build
```

After a successful build, the output will be in the `./dist/` directory:

```
dist/
├── index.html
├── assets/
│   ├── index-xxxxxxxx.js
│   ├── index-xxxxxxxx.css
│   └── ...
└── favicon.ico
```

---

### 2. Create S3 Bucket for Static Hosting

1. Navigate to **AWS Console** → **S3** → **Create bucket**.

| Field                   | Value                                                       |
| ----------------------- | ----------------------------------------------------------- |
| Bucket name             | `shopsflow-frontend-<your-account-id>`                      |
| AWS Region              | `ap-southeast-1`                                            |
| Block all public access | **Enable** (CloudFront uses OAC — not direct public access) |
| Bucket versioning       | Disable                                                     |

2. Click **Create bucket**.

![S3 bucket shopsflow-fe với các file đã được upload thành công](/images/5-Workshop/5.4-S3-onprem/s3-files-uploaded.jpg)

{{% notice note %}}
**Do not** enable "Static website hosting" on the S3 bucket — CloudFront will use **Origin Access Control (OAC)** to fetch content from S3 via a private connection. Public website hosting is not needed.
{{% /notice %}}

---

### 3. Upload Build Files to S3

Using the AWS CLI:

```bash
# Sync the dist/ folder to the S3 bucket
aws s3 sync ./dist s3://shopsflow-frontend-<your-account-id>/ \
  --delete \
  --region ap-southeast-1

# Verify upload
aws s3 ls s3://shopsflow-frontend-<your-account-id>/ --human-readable
```

Expected output:

```
2026-06-15 10:00:00    1.2 KiB index.html
2026-06-15 10:00:00    356.7 KiB assets/index-xxxxxxxx.js
2026-06-15 10:00:00     24.1 KiB assets/index-xxxxxxxx.css
```

**All frontend assets are now stored in S3 and ready to be served through CloudFront.**

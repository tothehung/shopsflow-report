---
title: "Build & Upload Frontend lên S3"
date: 2026-06-15
weight: 1
chapter: false
pre: " <b> 5.4.1 </b> "
---

Ở bước này, bạn sẽ build production bundle **React + Vite** và upload các file tĩnh lên **Amazon S3**. S3 sẽ đóng vai trò origin cho CloudFront CDN distribution.

---

### 1. Build Production Bundle

Trên máy cục bộ, trong thư mục `frontend/` của project Shopsflow:

```bash
cd shopsflow/frontend

# Cài đặt toàn bộ dependencies
npm install

# Cấu hình ALB DNS làm API backend URL
# Thay <ALB_DNS_NAME> bằng giá trị từ Mục 5.3
echo "VITE_API_BASE_URL=http://<ALB_DNS_NAME>" > .env.production
echo "VITE_APP_ENV=production" >> .env.production

# Build production bundle
npm run build
```

Sau khi build thành công, kết quả nằm trong thư mục `./dist/`:
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

### 2. Tạo S3 Bucket để lưu trữ tĩnh

1. Truy cập **AWS Console** → **S3** → **Create bucket**.

| Trường | Giá trị |
|---|---|
| Bucket name | `shopsflow-frontend-<your-account-id>` |
| AWS Region | `ap-southeast-1` |
| Block all public access | ✅ **Bật** (CloudFront dùng OAC — không cần public access trực tiếp) |
| Bucket versioning | Tắt |

2. Click **Create bucket**.

{{% notice note %}}
**Không** bật "Static website hosting" trên S3 bucket — CloudFront sẽ dùng **Origin Access Control (OAC)** để lấy nội dung từ S3 qua kết nối riêng tư. Không cần public website hosting.
{{% /notice %}}

---

### 3. Upload file Build lên S3

Dùng AWS CLI:
```bash
# Đồng bộ thư mục dist/ lên S3 bucket
aws s3 sync ./dist s3://shopsflow-frontend-<your-account-id>/ \
  --delete \
  --region ap-southeast-1

# Kiểm tra upload
aws s3 ls s3://shopsflow-frontend-<your-account-id>/ --human-readable
```

Kết quả mong đợi:
```
2026-06-15 10:00:00    1.2 KiB index.html
2026-06-15 10:00:00    356.7 KiB assets/index-xxxxxxxx.js
2026-06-15 10:00:00     24.1 KiB assets/index-xxxxxxxx.css
```

✅ **Toàn bộ file frontend đã được lưu trên S3 và sẵn sàng phân phối qua CloudFront.**

---
title: "S3 bucket cho frontend"
date: 2026-11-13
weight: 9
chapter: false
---

Tạo S3 bucket riêng tư để lưu bản build React. CloudFront sẽ truy cập bucket qua Origin Access Control.

## Bước 1: Tạo bucket

Mở **S3 → Buckets → Create bucket**.

![Mở trang tạo S3 bucket](/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/1-open-s3-dashboard-create-bucket.png)

## Bước 2: Cấu hình bucket

```text
Bucket name: nexusoms-demo-frontend-<ACCOUNT_ID>
AWS Region: ap-southeast-1
Block all public access: Enabled
```

Thay `<ACCOUNT_ID>` bằng Account ID của bạn. Giữ encryption mặc định và không bật **Static website hosting**.

![Cấu hình S3 bucket](/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/2-configure-bucket.png)

## Bước 3: Hoàn tất

Chọn **Create bucket**.

![Tạo S3 bucket](/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/3-click-create-bucket.png)

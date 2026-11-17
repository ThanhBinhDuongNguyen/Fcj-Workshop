---
title: "Frontend S3 bucket"
date: 2026-11-13
weight: 9
chapter: false
---

Create a private S3 bucket for the React build. CloudFront will access it through Origin Access Control.

## Step 1: Create the bucket

Open **S3 → Buckets → Create bucket**.

![Open the S3 bucket creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/1-open-s3-dashboard-create-bucket.png)

## Step 2: Configure the bucket

```text
Bucket name: nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
AWS Region: ap-southeast-1
Block all public access: Enabled
```

Replace `<AWS_ACCOUNT_ID>` with your account ID. Keep the default encryption and do not enable **Static website hosting**.

![Configure the S3 bucket](/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/2-configure-bucket.png)

## Step 3: Finish

Choose **Create bucket**.

![Create the S3 bucket](/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/3-click-create-bucket.png)

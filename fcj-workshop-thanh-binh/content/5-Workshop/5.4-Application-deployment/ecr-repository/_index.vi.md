---
title: "ECR repository"
date: 2026-11-17
weight: 1
chapter: false
---

## 1. Mở Amazon ECR

Chọn Region **Asia Pacific (Singapore) / ap-southeast-1**.

Mở **Amazon ECR → Private registry → Repositories → Create repository**.

![Mở ECR và tạo repository](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/1-open-ecr-dashboard-and-create-repository.png)

## 2. Cấu hình repository

Nhập:

```text
Visibility: Private
Repository name: nexusoms-demo-api
Image tag mutability: Immutable
Scan on push: Enabled
Encryption: AES-256
```

![Cấu hình private repository](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/2-configure-private-repository.png)

## 3. Tạo repository

Kiểm tra lại cấu hình rồi chọn **Create repository**.

![Kiểm tra mã hóa và tạo repository](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/3-confirm-encryption-and-create-repository.png)

## 4. Sao chép Repository URI

Mở `nexusoms-demo-api` và chọn **Copy URI**.

![Sao chép ECR Repository URI](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecr-repository/4-copy-ecr-repository-uri.png)

Lưu URI để build và push backend image:

```text
<ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/nexusoms-demo-api
```

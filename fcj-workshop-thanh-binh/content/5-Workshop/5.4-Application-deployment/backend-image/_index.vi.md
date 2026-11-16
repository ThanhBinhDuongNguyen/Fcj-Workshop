---
title: "Build và push backend image"
date: 2026-11-15
weight: 2
chapter: false
---

Mở PowerShell tại thư mục gốc của repository.

## 1. Đăng nhập ECR và build image

```powershell
$Region = "ap-southeast-1"
$AccountId = aws sts get-caller-identity `
  --profile nexusoms-demo `
  --query Account `
  --output text
$EcrRepositoryUri = "$AccountId.dkr.ecr.$Region.amazonaws.com/nexusoms-demo-api"
$Registry = $EcrRepositoryUri.Split('/')[0]
$ImageTag = "v1-initial"

aws ecr get-login-password `
  --profile nexusoms-demo `
  --region $Region |
  docker login --username AWS --password-stdin $Registry

docker build `
  --file backend/Dockerfile `
  --tag "nexusoms-demo-api:$ImageTag" `
  backend
```

Kết quả login phải là `Login Succeeded` và Docker build phải hoàn tất.

![Đăng nhập ECR và build backend image](/images/5-Workshop/nexusoms/5.4-Application-deployment/backend-image/1-bootstrap-login-and-build-backend.png)

## 2. Tag và push image

```powershell
docker tag `
  "nexusoms-demo-api:$ImageTag" `
  "$EcrRepositoryUri`:$ImageTag"

docker push "$EcrRepositoryUri`:$ImageTag"

$BackendImageUri = "$EcrRepositoryUri`:$ImageTag"
$BackendImageUri
```

![Push backend image lên ECR](/images/5-Workshop/nexusoms/5.4-Application-deployment/backend-image/2-bootstrap-push-image-to-ecr.png)

Ghi lại `$BackendImageUri` để nhập vào Task Definition.

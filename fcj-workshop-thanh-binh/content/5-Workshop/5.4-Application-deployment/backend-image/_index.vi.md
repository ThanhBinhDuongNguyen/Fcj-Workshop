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

aws ecr get-login-password --profile nexusoms-demo --region $Region | docker login --username AWS --password-stdin $Registry

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

## 3. Xử lý lỗi Docker credentials trên Windows

Nếu `docker login` báo `The stub received bad data` hoặc `docker push` báo `no basic auth credentials`, không build lại image. Chạy block sau để push bằng Docker config tạm:

Giá trị `auth` trong file tạm chỉ được mã hóa Base64, không phải mã hóa bảo mật. File được tạo ngoài repository và bị xóa trong `finally`.

```powershell
$DockerConfigDir = Join-Path $env:TEMP "nexusoms-docker-$([guid]::NewGuid())"

try {
  New-Item -ItemType Directory -Path $DockerConfigDir -Force | Out-Null
  $EcrPassword = aws ecr get-login-password --profile nexusoms-demo --region $Region
  $Auth = [Convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes("AWS:$EcrPassword"))
  @{ auths = @{ $Registry = @{ auth = $Auth } } } |
    ConvertTo-Json -Depth 4 |
    Set-Content -LiteralPath (Join-Path $DockerConfigDir "config.json") -Encoding utf8

  docker --config $DockerConfigDir push "$EcrRepositoryUri`:$ImageTag"
} finally {
  Remove-Item -LiteralPath $DockerConfigDir -Recurse -Force -ErrorAction SilentlyContinue
}
```

Mở ECR repository và xác nhận image tag `v1-initial` cùng digest đã xuất hiện.

---
title: "Build và push backend image"
date: 2026-11-13
weight: 2
chapter: false
---

Dùng tag bất biến duy nhất cho mỗi lần build. Sau khi push, copy image URI đầy đủ gồm tag để tạo task definition.

## 8.2. Đăng nhập Docker vào ECR và push tag duy nhất

Chạy trong PowerShell từ repository root. Thay &lt;ECR_REPOSITORY_URI&gt; bằng URI đã copy. Mỗi lần build phải dùng tag chưa từng dùng trong repository immutable, ví dụ ngày build và số thứ tự mới.

```
$Region = 'ap-southeast-1'
$EcrRepositoryUri = '992275841761.dkr.ecr.ap-southeast-1.amazonaws.com/nexusoms-demo-api'
$Registry = $EcrRepositoryUri.Split('/')[0]
$ImageTag = 'vYYYY-MM-DD-01'
```

```
aws ecr get-login-password --region $Region |
  docker login --username AWS --password-stdin $Registry
```

```
Set-Location E:\Noxus-aws\nexus-aws
docker build -f .\backend\Dockerfile -t "nexusoms-demo-api:$ImageTag" .\backend
docker tag "nexusoms-demo-api:$ImageTag" "$EcrRepositoryUri`:$ImageTag"
docker push "$EcrRepositoryUri`:$ImageTag"
```

Kết quả login phải là Login Succeeded. Sau khi push, ECR → repository → Images phải có tag vừa tạo và digest. Ghi lại **full image URI**, gồm tag. Khi lỗi Docker API, kiểm tra Docker Desktop có chạy Linux Engine; khi ECR no basic auth credentials, chạy lại lệnh login đúng region/account.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**ECR repository**

```text
$EcrRepositoryUri = '<AWS_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/nexusoms-demo-api'
```

**Build và push backend image - giá trị 2**

```text
$ImageTag = 'v<YYYY-MM-DD>-01'
```

**Lệnh PowerShell**

```text
docker build -f .\backend\Dockerfile -t "nexusoms-demo-api:$ImageTag" .\backend
```

**Lệnh PowerShell**

```text
docker tag "nexusoms-demo-api:$ImageTag" "$EcrRepositoryUri`:$ImageTag"
```

**Lệnh PowerShell**

```text
docker push "$EcrRepositoryUri`:$ImageTag"
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Set up variable + Docker build BE

Trên AWS Console, thực hiện **Set up variable + Docker build BE** như hình dưới đây.

**Giá trị dùng ở bước này**

**Lệnh PowerShell**

```text
docker build -f .\backend\Dockerfile -t "nexusoms-demo-api:$ImageTag" .\backend
```

**Lệnh PowerShell**

```text
docker tag "nexusoms-demo-api:$ImageTag" "$EcrRepositoryUri`:$ImageTag"
```

**Lệnh PowerShell**

```text
docker push "$EcrRepositoryUri`:$ImageTag"
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/backend-image/1-set-up-variable-docker-build-be.png" title="Set up variable + Docker build BE" >}}

### Bước 2 - Push Image to ECR Repository

Trên AWS Console, thực hiện **Push Image to ECR Repository** như hình dưới đây.

**Giá trị dùng ở bước này**

**ECR repository**

```text
$EcrRepositoryUri = '<AWS_ACCOUNT_ID>.dkr.ecr.ap-southeast-1.amazonaws.com/nexusoms-demo-api'
```

**Build và push backend image - giá trị 2**

```text
$ImageTag = 'v<YYYY-MM-DD>-01'
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/backend-image/2-push-image-to-ecr-repository.png" title="Push Image to ECR Repository" >}}

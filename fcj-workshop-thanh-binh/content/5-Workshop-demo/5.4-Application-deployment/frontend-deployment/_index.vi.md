---
title: "Build và upload frontend"
date: 2026-11-13
weight: 11
chapter: false
---

Frontend production phải gọi API bằng đường dẫn tương đối `/api`; không build bằng URL localhost hoặc ALB trực tiếp.

## 11. Build và upload frontend đúng endpoint

Frontend API client nối VITE_API_BASE_URL trực tiếp với các path đã có /api/v1/.... Vì vậy biến phải là domain CloudFront gốc, không có /api:

```
Đúng:  VITE_API_BASE_URL=https://dxxxxxxxxxxxx.cloudfront.net
Sai:   VITE_API_BASE_URL=/api
Sai:   VITE_API_BASE_URL=https://dxxxxxxxxxxxx.cloudfront.net/api
Sai:   VITE_API_BASE_URL=http://localhost:5145
```

Đặt ba biến dưới đây trong PowerShell, thay domain, URL và publishable key bằng giá trị thật:

```
Set-Location E:\Noxus-aws\nexus-aws\frontend
$env:VITE_DATA_MODE = 'supabase'
$env:VITE_API_BASE_URL = 'https://<CLOUDFRONT_DOMAIN>'
$env:VITE_SUPABASE_URL = 'https://<PROJECT_REF>.supabase.co'
$env:VITE_SUPABASE_ANON_KEY = '<SUPABASE_PUBLISHABLE_KEY>'
npm ci
npm run build
```

Vite nhúng VITE_\* vào JavaScript lúc build; thay Environment Variables trong hosting sau đó không sửa được file đã build. Nếu đổi API base URL hoặc Supabase URL/key, build và upload lại frontend.

Sau build, từ thư mục frontend chạy:

```
$BucketName = '<FRONTEND_BUCKET_NAME>'
$DistributionId = '<CLOUDFRONT_DISTRIBUTION_ID>'
aws s3 sync .\dist\ "s3://$BucketName/" --delete --region ap-southeast-1
aws cloudfront create-invalidation --distribution-id $DistributionId --paths '/*' --region us-east-1
```

aws cloudfront là global service; nếu CLI báo region, dùng us-east-1 hoặc bỏ --region. --delete chỉ dùng khi bucket dành riêng cho frontend này vì nó xóa object cũ không còn trong dist. Kiểm tra S3 có index.html và assets/; CloudFront → Invalidations phải hoàn tất trước khi test.

Ảnh cần chụp: 08-s3/frontend-assets/frontend-assets.png (index.html và assets), 09-cloudfront/invalidation/invalidation-complete.png (status Completed).

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Build và upload frontend - giá trị 1**

```text
VITE_API_BASE_URL=/api
```

**VITE_SUPABASE_URL=https**

```text
VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
```

**Supabase value**

```text
VITE_SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_KEY>
```

**Lệnh PowerShell**

```text
npm ci
```

**Lệnh PowerShell**

```text
npm run build
```

**Lệnh PowerShell**

```text
aws s3 sync .\dist "s3://nexusoms-demo-frontend-<AWS_ACCOUNT_ID>" --delete
```

**Lệnh PowerShell**

```text
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths "/*"
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Set up variable + run build project

Trên AWS Console, thực hiện **Set up variable + run build project** như hình dưới đây.

**Giá trị dùng ở bước này**

**Build và upload frontend - giá trị 1**

```text
VITE_API_BASE_URL=/api
```

**VITE_SUPABASE_URL=https**

```text
VITE_SUPABASE_URL=https://<PROJECT_REF>.supabase.co
```

**Supabase value**

```text
VITE_SUPABASE_ANON_KEY=<SUPABASE_PUBLISHABLE_KEY>
```

**Lệnh PowerShell**

```text
npm ci
```

**Lệnh PowerShell**

```text
npm run build
```

**Lệnh PowerShell**

```text
aws s3 sync .\dist "s3://nexusoms-demo-frontend-<AWS_ACCOUNT_ID>" --delete
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/1-set-up-variable-run-build-project.png" title="Set up variable + run build project" >}}

### Bước 2 - Push to S3

Trên AWS Console, thực hiện **Push to S3** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/2-push-to-s3.png" title="Push to S3" >}}

### Bước 3 - Create invalidation CloudFront

Trên AWS Console, thực hiện **Create invalidation CloudFront** như hình dưới đây.

**Giá trị dùng ở bước này**

**Lệnh PowerShell**

```text
aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths "/*"
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-deployment/3-create-invalidation-cloudfront.png" title="Create invalidation CloudFront" >}}

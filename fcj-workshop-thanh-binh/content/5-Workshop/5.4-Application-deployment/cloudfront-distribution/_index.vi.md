---
title: "CloudFront Distribution"
date: 2026-11-13
weight: 11
chapter: false
---

Tạo một CloudFront Distribution với S3 làm origin mặc định cho frontend và ALB làm origin cho `/api/*`.

## Bước 1: Tạo distribution

Mở **CloudFront → Distributions → Create distribution**.

![Mở trang tạo distribution](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/1-open-cloudfront-dashboard-create-distribution.png)

Nhập tên distribution:

```text
nexusoms-demo-web
```

![Cấu hình distribution](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/2-configure-distribution-options.png)

## Bước 2: Chọn S3 origin cho frontend

Chọn **Amazon S3**, sau đó chọn bucket `nexusoms-demo-frontend-<ACCOUNT_ID>`.

```text
Origin access: Origin access control settings
Viewer protocol policy: Redirect HTTP to HTTPS
Default root object: index.html
```

Giữ **Block all public access** của bucket ở trạng thái bật.

Sau khi tạo distribution, mở thông báo S3 bucket policy của CloudFront và chọn **Copy policy** nếu Console chưa tự cập nhật quyền OAC. Thêm policy vào đúng frontend bucket.

![Chọn S3 origin](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/3-choose-s3-type-mapping-to-s3-fe.png)

Chọn **Next**.

![Tiếp tục cấu hình distribution](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/4-click-next.png)

## Bước 3: Hoàn tất distribution

Trong phần bảo vệ WAF, chọn **Do not enable security protections**.

![Không bật WAF cho workshop](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/5-choose-do-not-enable-for-demo.png)

Chọn **Create distribution** và chờ trạng thái **Deployed**.

![Tạo distribution](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/6-click-create-distribution.png)

## Bước 4: Thêm ALB origin

Mở distribution vừa tạo, chọn **Origins → Create origin**.

![Mở phần tạo origin](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/7-choose-origins-create-origin.png)

Nhập cấu hình:

```text
Origin domain: <ALB_DNS_NAME>
Origin name: nexusoms-demo-alb-origin
Origin path: để trống
Protocol: HTTP only
HTTP port: 80
```

`Origin domain` chỉ chứa DNS name của ALB, không thêm `http://` hoặc `https://`.

![Cấu hình ALB origin](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/8-configure-setting-for-origin.png)

## Bước 5: Tạo behavior cho API

Chọn **Behaviors → Create behavior**.

![Mở phần tạo behavior](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/9-choose-behavior-tab-create-behavior.png)

Nhập cấu hình:

```text
Path pattern: /api/*
Origin: nexusoms-demo-alb-origin
Viewer protocol policy: Redirect HTTP to HTTPS
Allowed HTTP methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
Cache policy: CachingDisabled
Origin request policy: AllViewerExceptHostHeader
```

![Cấu hình behavior cho API](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/10-configure-behavior-setting.png)

![Chọn Origin request policy](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/11-choose-origin-request-policy.png)

Chọn **Create behavior**. Kiểm tra `/api/*` đang trỏ đến ALB origin và tắt cache.

Nếu ô Path pattern báo `globPattern invalid`, xóa nội dung và gõ lại `/api/*` bằng bàn phím.

## Bước 6: Ghi lại thông tin CloudFront

Ghi lại **Distribution ID** và **Distribution domain name** để cấu hình GitHub Actions.

```text
Distribution ID: E...
Distribution domain name: d....cloudfront.net
```

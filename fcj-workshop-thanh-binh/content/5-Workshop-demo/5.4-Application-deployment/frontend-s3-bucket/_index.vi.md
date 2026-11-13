---
title: "S3 bucket cho frontend"
date: 2026-11-13
weight: 8
chapter: false
---

Bucket frontend phải riêng tư. CloudFront truy cập bucket qua Origin Access Control; không bật static website hosting công khai.

## 10.1. S3 frontend bucket riêng tư

1\. Mở **S3 → Buckets → Create bucket**.

2\. Bucket name phải duy nhất toàn cầu, ví dụ nexusoms-demo-frontend-&lt;AWS_ACCOUNT_ID&gt;; thay placeholder bằng account ID của bạn.

3\. Region ap-southeast-1.

4\. Giữ **Block all public access** bật. Không tắt để "test cho nhanh".

5\. Bật versioning nếu muốn lưu bản cũ; encryption SSE-S3 mặc định; tạo bucket.

6\. Không bật Static website hosting. CloudFront dùng S3 REST origin riêng tư qua OAC.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Frontend resource name**

```text
nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
```

**Block all public access**

```text
Block all public access: Enabled
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open S3 dashboard + Create bucket

Trên AWS Console, thực hiện **Open S3 dashboard + Create bucket** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/1. Open S3 dashboard + Create bucket.png" title="Open S3 dashboard + Create bucket" >}}

### Bước 2 - Configure bucket

Trên AWS Console, thực hiện **Configure bucket** như hình dưới đây.

**Giá trị dùng ở bước này**

**Frontend resource name**

```text
nexusoms-demo-frontend-<AWS_ACCOUNT_ID>
```

**Block all public access**

```text
Block all public access: Enabled
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/2. Configure bucket.png" title="Configure bucket" >}}

### Bước 3 - Click create bucket

Trên AWS Console, thực hiện **Click create bucket** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/frontend-s3-bucket/3. Click create bucket.png" title="Click create bucket" >}}

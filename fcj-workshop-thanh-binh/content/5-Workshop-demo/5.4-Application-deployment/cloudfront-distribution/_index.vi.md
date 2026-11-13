---
title: "CloudFront Distribution"
date: 2026-11-13
weight: 9
chapter: false
---

Distribution có S3 làm default origin và ALB làm origin cho `/api/*`. Forward Authorization/JWT nhưng không forward Host header của CloudFront đến ALB.

## 10.2. Tạo distribution với S3 làm origin mặc định

1\. Mở **CloudFront → Distributions → Create distribution**.

2\. Distribution name: nexusoms-demo-web; chọn **Single website configuration**.

3\. Domain managed by Route 53: để trống/skip. Ta dùng domain cloudfront.net; không cần mua domain hoặc Route 53.

4\. Ở **Specify origin**, chọn **Amazon S3** và bucket vừa tạo.

5\. Bật **Allow private S3 bucket access to CloudFront** / tạo OAC được khuyến nghị. Chấp nhận để Console cập nhật bucket policy nếu được hỏi.

6\. Origin settings và cache settings giữ đề xuất cho file tĩnh.

7\. Viewer protocol policy chọn Redirect HTTP to HTTPS; default root object index.html nếu trường này có trong bước đầu hoặc bổ sung trong General sau khi tạo.

8\. WAF: với demo tiết kiệm, chọn **Do not enable security protections**. WAF có thể phát sinh phí; không bật nhầm protection khi chưa kiểm tra ước tính.

9\. Tạo distribution. Chờ status **Deployed**.

S3 origin sẽ có dạng bucket regional domain hoặc tên origin do CloudFront tạo. OAC phải được gắn với S3 origin. S3 vẫn giữ Block Public Access bật.

## 10.3. Thêm ALB làm origin thứ hai

Đây là bước phải làm **trước khi tạo \`/api/\*\` behavior**. Behavior chỉ liệt kê những origin đã tồn tại. Nếu dropdown origin chỉ hiện S3, ALB origin chưa được tạo - không cố chọn S3 cho API.

1\. Mở distribution → tab **Origins** → **Create origin**.

2\. Chọn origin type **Elastic Load Balancer** nếu Console cho chọn loại này; nếu Console yêu cầu domain, paste ALB DNS đã copy, ví dụ dạng nexusoms-demo-alb-....ap-southeast-1.elb.amazonaws.com.

3\. Không gõ https:// và không thêm path vào domain origin.

4\. Origin protocol: **HTTP only**, HTTP port 80 cho cấu hình demo hiện tại.

5\. Origin name nexusoms-demo-alb-origin; lưu origin.

6\. Xác nhận tab Origins có hai origin: S3 frontend và ALB.

## 10.4. Tạo /api/\* behavior nối đến ALB

1\. Distribution → **Behaviors → Create behavior**.

2\. Path pattern: /api/\*.

3\. Origin: chọn nexusoms-demo-alb-origin (không chọn S3).

4\. Viewer protocol policy: **Redirect HTTP to HTTPS**.

5\. Allowed HTTP methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE.

6\. Cache policy: **CachingDisabled**.

7\. Origin request policy: **AllViewerExceptHostHeader** để chuyển tiếp Authorization header/JWT và query/request headers cần thiết, không chuyển Host của browser thành Host origin.

8\. Tạo behavior. Behavior /api/\* phải đứng trước behavior Default (\*); CloudFront tự áp dụng path match cụ thể.

**Kiểm tra:** trong Behaviors có Path pattern /api/\* → ALB origin, cache disabled. Nếu dropdown không có ALB, quay lại Origins và hoàn thành mục 10.3. Không tạo behavior API trỏ S3.

## 10.6. Ghi lại CloudFront domain và ID

Distribution → General. Copy riêng:

- **Distribution domain name**, ví dụ dxxxxxxxxxxxx.cloudfront.net; dùng trong website URL và frontend API base URL.
- **Distribution ID**, mã khác domain; dùng khi tạo invalidation.

Không dùng ALB DNS làm website URL, không dùng S3 URL làm website URL, không dùng Distribution ID làm URL.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**CloudFront → Distributions → Create distribution**

```text
nexusoms-demo-web
```

**Default root object**

```text
Default root object: index.html
```

**HTTP only**

```text
nexusoms-demo-alb-origin
```

**Path pattern**

```text
Path pattern: /api/*
```

**Cache policy**

```text
Cache policy: CachingDisabled
```

**Origin request policy**

```text
Origin request policy: AllViewerExceptHostHeader
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open CloudFront dashboard + Create distribution

Trên AWS Console, thực hiện **Open CloudFront dashboard + Create distribution** như hình dưới đây.

**Giá trị dùng ở bước này**

**CloudFront → Distributions → Create distribution**

```text
nexusoms-demo-web
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/1. Open CloudFront dashboard + Create distribution.png" title="Open CloudFront dashboard + Create distribution" >}}

### Bước 2 - Configure distribution options

Trên AWS Console, thực hiện **Configure distribution options** như hình dưới đây.

**Giá trị dùng ở bước này**

**Default root object**

```text
Default root object: index.html
```

**Path pattern**

```text
Path pattern: /api/*
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/2. Configure distribution options.png" title="Configure distribution options" >}}

### Bước 3 - Choose S3 type + mapping to S3 FE

Trên AWS Console, thực hiện **Choose S3 type + mapping to S3 FE** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/3. Choose S3 type + mapping to S3 FE.png" title="Choose S3 type + mapping to S3 FE" >}}

### Bước 4 - Click next

Trên AWS Console, thực hiện **Click next** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/4. Click next.png" title="Click next" >}}

### Bước 5 - Choose Do not enable for demo

Trên AWS Console, thực hiện **Choose Do not enable for demo** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/5. Choose Do not enable for demo.png" title="Choose Do not enable for demo" >}}

### Bước 6 - Click Create distribution

Trên AWS Console, thực hiện **Click Create distribution** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/6. Click Create distribution.png" title="Click Create distribution" >}}

### Bước 7 - Choose origins + Create origin

Trên AWS Console, thực hiện **Choose origins + Create origin** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/7. Choose origins + Create origin.png" title="Choose origins + Create origin" >}}

### Bước 8 - Configure setting for origin

Trên AWS Console, thực hiện **Configure setting for origin** như hình dưới đây.

**Giá trị dùng ở bước này**

**HTTP only**

```text
nexusoms-demo-alb-origin
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/8. Configure setting for origin.png" title="Configure setting for origin" >}}

### Bước 9 - Choose Behavior tab + Create behavior

Trên AWS Console, thực hiện **Choose Behavior tab + Create behavior** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/9. Choose Behavior tab + Create behavior.png" title="Choose Behavior tab + Create behavior" >}}

### Bước 10 - Configure behavior setting

Trên AWS Console, thực hiện **Configure behavior setting** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/10. Configure behavior setting.png" title="Configure behavior setting" >}}

### Bước 11 - Choose origin request policy

Trên AWS Console, thực hiện **Choose origin request policy** như hình dưới đây.

**Giá trị dùng ở bước này**

**Cache policy**

```text
Cache policy: CachingDisabled
```

**Origin request policy**

```text
Origin request policy: AllViewerExceptHostHeader
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/11. Choose origin request policy.png" title="Choose origin request policy" >}}

---
title: "CloudFront Function cho SPA"
date: 2026-11-13
weight: 12
chapter: false
---

Tạo CloudFront Function để các đường dẫn React không có đuôi file được phục vụ bằng `/index.html`.

## Bước 1: Tạo function

Mở **CloudFront → Functions → Create function**.

![Mở trang tạo function](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/1-open-functions-dashboard-create-function.png)

Nhập cấu hình:

```text
Function name: nexusoms-demo-spa-routing
Runtime: cloudfront-js-2.0
```

![Cấu hình function](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/2-configure-function-details.png)

## Bước 2: Thêm code rewrite SPA

Xóa code mẫu và dán toàn bộ đoạn sau:

```javascript
function handler(event) {
    var request = event.request;
    var uri = request.uri;

    // Chuyển route SPA về index.html nhưng giữ nguyên file tĩnh.
    if (uri === "/" || uri.indexOf(".") === -1) {
        request.uri = "/index.html";
    }

    return request;
}
```

![Dán code vào Build section](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/3-paste-code-build-section.png)

## Bước 3: Publish function

Chọn **Save changes**, sau đó chọn **Publish**.

![Publish function](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/4-publish-function.png)

## Bước 4: Gắn function vào distribution

Mở distribution → **Behaviors** → chọn **Default (*) → Edit**.

![Mở Default behavior](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/5-open-distribution-detail-choose-behaviors-edit.png)

Trong **Function associations**, chọn:

```text
Event type: Viewer request
Function type: CloudFront Functions
Function ARN/Name: nexusoms-demo-spa-routing
Stage: LIVE
```

Không gắn function vào behavior `/api/*`.

![Gắn function vào Viewer request](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/6-configure-function-associations.png)

## Bước 5: Kiểm tra

Chờ distribution chuyển sang **Deployed**. Sao chép domain name và mở đường dẫn `/app?page=dashboard`.

![Sao chép Distribution domain name](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/7-copy-distribution-domain-name.png)

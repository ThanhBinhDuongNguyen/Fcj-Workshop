---
title: "CloudFront Function cho SPA"
date: 2026-11-13
weight: 10
chapter: false
---

Function rewrite các đường dẫn frontend không có đuôi file về `/index.html`, nhưng giữ nguyên `/api/*`, file tĩnh và query string.

## 10.5. SPA deep link bằng CloudFront Function

React/Vite chỉ có index.html. Khi mở thẳng /app?page=dashboard, S3 tìm object tên /app và có thể trả XML AccessDenied. CloudFront Function ở **Viewer request của Default (\*) behavior** đổi URI frontend không có đuôi file thành /index.html. Query ?page=dashboard được giữ nguyên.

1\. CloudFront → **Functions → Create function**.

2\. Function name: nexusoms-demo-spa-routing; runtime JavaScript 2.0 nếu Console hỏi.

3\. Thay code mẫu bằng đoạn sau:

```
function handler(event) {
    var request = event.request;
    var uri = request.uri;
```

```
    // Route SPA như /app hoặc /app?page=dashboard về tài liệu React.
    // File tĩnh như /assets/index-abc.js vẫn giữ nguyên URI.
    if (uri === "/" || uri.indexOf(".") === -1) {
        request.uri = "/index.html";
    }
```

```
    return request;
}
```

4\. Chọn **Save changes**, sau đó **Publish function**. Chọn stage **LIVE** khi gắn vào distribution.

5\. Distribution → **Behaviors** → chọn Default (\*) → **Edit**.

6\. Phần **Function associations** → Viewer request → CloudFront Functions → chọn nexusoms-demo-spa-routing, stage **LIVE**; lưu.

7\. Không gắn function này lên /api/\* behavior. Không đổi ALB/API origin.

8\. Chờ distribution status **Deployed**, rồi tạo invalidation /\* ở mục Invalidations.

Không cấu hình custom error 403/404 thành trang /index.html trả HTTP 200 khi đã dùng Function. Custom error áp dụng cho lỗi thật từ origin và có thể biến lỗi API/S3 thành HTML 200, khiến trình duyệt báo lỗi khó hiểu. Function xử lý đường dẫn SPA trước khi S3 tìm object, còn lỗi quyền thật vẫn phải hiện ra để xử lý đúng.

Ảnh cần chụp: 09-cloudfront/distribution/distribution-config.png thể hiện hai origin, /api/\* trỏ ALB, Default (\*) trỏ S3 và Function LIVE ở Viewer request. 09-cloudfront/spa-routing/function-live.png thể hiện function Published/LIVE; không chụp secret.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Functions → Create function**

```text
nexusoms-demo-spa-routing
```

**Runtime**

```text
Runtime: JavaScript 2.0
```

**Event**

```text
Event: Viewer request
```

**Behavior**

```text
Behavior: Default (*)
```

**Stage**

```text
Stage: LIVE
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Functions dashboard + Create function

Trên AWS Console, thực hiện **Open Functions dashboard + Create function** như hình dưới đây.

**Giá trị dùng ở bước này**

**Functions → Create function**

```text
nexusoms-demo-spa-routing
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/1. Open Functions dashboard + Create function.png" title="Open Functions dashboard + Create function" >}}

### Bước 2 - Configure function details

Trên AWS Console, thực hiện **Configure function details** như hình dưới đây.

**Giá trị dùng ở bước này**

**Runtime**

```text
Runtime: JavaScript 2.0
```

**Event**

```text
Event: Viewer request
```

**Behavior**

```text
Behavior: Default (*)
```

**Stage**

```text
Stage: LIVE
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/2. Configure function details.png" title="Configure function details" >}}

### Bước 3 - Paste code build section

Trên AWS Console, thực hiện **Paste code build section** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/3. Paste code build section.png" title="Paste code build section" >}}

### Bước 4 - Publish function

Trên AWS Console, thực hiện **Publish function** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/4. Publish function.png" title="Publish function" >}}

### Bước 5 - Open distribution detail + choose behaviors + edit

Trên AWS Console, thực hiện **Open distribution detail + choose behaviors + edit** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/5. Open distribution detail + choose behaviors + edit.png" title="Open distribution detail + choose behaviors + edit" >}}

### Bước 6 - Configure Function associations

Trên AWS Console, thực hiện **Configure Function associations** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/6. Configure Function associations.png" title="Configure Function associations" >}}

### Bước 7 - Copy Distribution domain name

Trên AWS Console, thực hiện **Copy Distribution domain name** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/7. Copy Distribution domain name.png" title="Copy Distribution domain name" >}}

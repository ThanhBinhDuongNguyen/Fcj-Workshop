---
title: "Kiểm thử end-to-end"
date: 2026-11-13
weight: 1
chapter: false
---

Kiểm tra từ CloudFront đến S3, ALB, ECS, Supabase và CloudWatch. Chẩn đoán lỗi dựa trên status code, target health và log thay vì tạo lại tài nguyên.

## 12. Supabase Auth và kiểm tra đăng nhập

1\. Supabase Dashboard → **Authentication → URL Configuration**.

2\. **Site URL** nhập https://&lt;CLOUDFRONT_DOMAIN&gt; - chỉ domain gốc, không /app hoặc ?page=....

3\. **Redirect URLs** thêm https://&lt;CLOUDFRONT_DOMAIN&gt;/\*. Giữ localhost redirect nếu vẫn phát triển local.

4\. Save.

5\. Mở tab ẩn danh https://&lt;CLOUDFRONT_DOMAIN&gt;/app. Đăng nhập bằng user Supabase đã có dòng hợp lệ trong public.profiles và role phù hợp.

6\. Mở DevTools → Network → request GET /api/v1/session/me.

**Đăng nhập thành công khi:** request URL là https://&lt;CLOUDFRONT_DOMAIN&gt;/api/v1/session/me - chỉ một đoạn /api; Request Headers có Authorization: Bearer ...; response HTTP 200 trả userId và roles.

Không gửi Bearer token cho người khác và không dán token vào tài liệu/chụp ảnh. Nếu token từng bị chia sẻ, đăng xuất các session có thể và đăng nhập lại.

## 13. Kiểm thử end-to-end trong ứng dụng

Thực hiện bằng user có role admin hoặc manager và data demo phù hợp.

Lưu các ảnh vào thư mục dịch vụ tương ứng dưới fcj-workshop-thanh-binh/content/5-Workshop/. Các tên file bên dưới giữ nguyên để ghép với cấu trúc workshop đã tạo.

1\. **Sản phẩm:** xác nhận có sản phẩm active, SKU nội bộ đúng, giá vốn đã có.

2\. **Kho:** mở Vận hành kho; chọn đúng warehouse; xác nhận sản phẩm có tồn khả dụng đủ cho quantity trong payload. Tồn khả dụng = tồn thực tế - đang giữ - không khả dụng theo dữ liệu hiện tại.

3\. **Kết nối sàn:** chọn/tạo channel connection đúng Shopee/TikTok Shop/Lazada; ghi lại tên tài khoản.

4\. **Ánh xạ SKU:** với channel connection đã chọn, tạo mapping từ SKU trên kênh đúng sang SKU nội bộ đúng. SKU payload phải khớp mapping; nhập SKU ngẫu nhiên thì mapping thất bại.

5\. **Mô phỏng webhook:** mở nút tròn ở góc dưới bên phải → Mô phỏng Flash Sale; chọn đúng tài khoản kênh và kho xuất; chọn SKU đã ánh xạ; nhập quantity dương và đơn giá; thêm vào payload.

6\. Bấm **Bắt đầu: Tạo đơn**. Quan sát process; chỉ bấm bước tiếp theo sau khi bước hiện tại báo thành công: **Tạo đơn & giữ tồn → Xác nhận đơn → Chuẩn bị hàng → Xuất kho & vận chuyển → Giao thành công**.

7\. Sau từng bước, mở Đơn hàng và Vận hành kho để đối chiếu order status và số tồn. Sau bước cuối mở Hiệu quả kênh và Lợi nhuận đơn để kiểm tra số liệu.

8\. Trong CloudWatch → Log groups → /ecs/nexusoms-demo-api, mở stream có timestamp vừa test; xác nhận không có unhandled exception.

Không gửi lại cùng một event/key để "thử thêm" nếu đơn đã xử lý; backend dùng idempotency và có thể trả kết quả replay/conflict. Tạo kịch bản đơn mới bằng simulator. Không chạy số lượng vượt tồn khả dụng.

Ảnh cần chụp: 10-verification/browser-login/live-homepage.png, 10-verification/webhook-flow/webhook-flow.png, 10-verification/cloudwatch-logs/api-logs.png. Che email, JWT, database strings và mọi secret.

## 14. Chẩn đoán lỗi theo bằng chứng

| **Triệu chứng**                                                       | **Ý nghĩa thường gặp**                                                                                         | **Kiểm tra và xử lý theo thứ tự**                                                                                                                                                                                        |
| --------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| XML AccessDenied khi mở /app?page=dashboard nhưng /app mở được        | S3 nhận URI /app thay vì /index.html, hoặc Function chưa LIVE/gắn đúng                                         | Function Published? Stage LIVE? Distribution → Default (\*) → Viewer request có Function? Distribution Deployed? Sau đó invalidation /\*; query string phải được giữ. Không sửa API behavior để xử lý lỗi này            |
| URL là /api/api/v1/session/me hoặc /api/api/v1/returns                | API base URL đã bị thêm /api, trong khi code endpoint có sẵn /api/v1                                           | Build frontend lại với VITE_API_BASE_URL=https://&lt;CLOUDFRONT_DOMAIN&gt; không có path; upload dist; invalidation /\*                                                                                                  |
| /api/v1/session/me 401                                                | API không chấp nhận bearer token hoặc token không được gửi                                                     | DevTools Network kiểm tra Authorization có hay không; login mới; kiểm tra Supabase URL/JWKS và clock/token. Không paste JWT vào chat/log                                                                                 |
| /api/v1/session/me 403 sau khi Supabase password login thành công     | Backend đã nhận diện user nhưng profile/role/quyền không hợp lệ, hoặc request nhầm API                         | Xem response body; kiểm tra public.profiles có đúng user ID/role; xác nhận request đi qua /api/\* tới ALB, không tới S3                                                                                                  |
| API trả 500 với ArgumentNullException (Host)                          | ConnectionStrings_\_Postgres rỗng/không được inject                                                            | Task Definition → revision đang chạy → Secrets: kiểm tra Key chính xác, ValueFrom chọn JSON key postgresConnectionString, execution role đọc đúng ARN; update service để tạo task mới                                    |
| Log Cannot load library libgssapi_krb5.so.2                           | Image runtime không có package Linux cần cho Npgsql, hoặc service vẫn chạy image cũ                            | Kiểm tra Dockerfile cài libgssapi-krb5-2; build/push tag immutable mới; tạo task definition revision trỏ full URI mới; update service và chờ deployment ổn định                                                          |
| NpgsqlException DNS/Host không resolve tới db.&lt;ref&gt;.supabase.co | VPC egress IPv4 không truy cập direct endpoint IPv6 hoặc host bị nhập sai                                      | Supabase → Connect → **Session pooler**; copy host/username hiện tại, port 5432; cập nhật postgresConnectionString; tạo task mới. Không đoán pooler host bằng cách ghép chuỗi                                            |
| GET /api/v1/returns trả 405 với Server: Kestrel và Allow: POST        | Request đã tới backend; đang chạy revision/image có action chỉ POST hoặc code đang deploy khác source hiện tại | So revision ECS đang active và image URI/tag; so source ReturnsController trên commit cần deploy; build tag mới, task definition revision mới, update service. Không sửa API behavior CloudFront nếu method đã được phép |
| CloudFront API trả 403/404/405/502/504                                | Behavior trỏ origin sai, method không được phép, hoặc API/ALB health                                           | /api/\* phải trỏ ALB; kiểm tra allowed methods; 405 + Server: Kestrel là backend route/method; 502/504 kiểm tra Target Group Healthy và ECS logs. Không tạo SPA fallback cho API                                         |
| Dropdown origin khi tạo behavior chỉ có S3                            | ALB chưa được thêm vào Origins                                                                                 | Distribution → Origins → Create origin, nhập/chọn ALB DNS, lưu; quay lại Behaviors và tạo /api/\*                                                                                                                        |
| Website vẫn là build cũ sau khi upload                                | Edge/browser đang giữ bản cũ hoặc build từ sai branch/biến                                                     | Xác nhận git rev-parse HEAD, xem S3 index.html/assets, tạo invalidation /\*, mở tab ẩn danh/clear cache                                                                                                                  |
| Webhook báo SystemActorId phải là UUID profile hợp lệ                 | Secret actor ID không khớp public.profiles.id                                                                  | Chạy truy vấn SQL phần 7.2, cập nhật JSON systemActorId, update service/force new deployment                                                                                                                             |
| Webhook 409                                                           | Backend từ chối trạng thái/dữ liệu trùng hoặc nghiệp vụ không hợp lệ; không đồng nghĩa request không tới API   | Đọc response body trong Network; xác nhận mapping đúng, còn tồn khả dụng, event/order chưa bị xử lý và đang bấm đúng bước kế tiếp. Tạo scenario mới thay vì replay event cũ                                              |
| Browser báo CORS khi frontend và API đang cùng CloudFront             | Frontend có thể build nhầm ALB/Render URL, khiến request thành cross-origin                                    | VITE_API_BASE_URL phải đúng CloudFront origin gốc; behavior /api/\* nối ALB. CORS không thay thế cấu hình origin và token forwarding                                                                                     |
| GET /health ở CloudFront trả S3 XML                                   | /health không thuộc API behavior /api/\*                                                                       | Đây không phải test endpoint qua CloudFront. Dùng Target Group health /health, ECS task health và CloudWatch logs                                                                                                        |

Khi gửi lỗi để nhờ kiểm tra, chỉ gửi: request URL đã che token, HTTP status, response body đã che dữ liệu cá nhân và dòng log quanh thời điểm lỗi. Không gửi Authorization, cookie, connection string hoặc secret value.

## 15. Cập nhật backend mà không làm lệch ECS

Mỗi lần thay backend:

1\. Kéo đúng source đã review, chạy tests.

2\. Build Docker image với tag mới chưa dùng.

3\. Push image tag mới lên ECR.

4\. ECS → Task definitions → tạo revision mới; sửa **Image URI** sang full ECR URI + tag mới. Giữ nguyên roles, port 8080, logging và bốn secret mapping.

5\. ECS → Cluster → Service → Update service; chọn task definition revision mới, cập nhật.

6\. Chờ task mới Running, service ổn định và Target Group Healthy. Kiểm tra CloudWatch log trước khi kết luận deploy xong.

Khi cập nhật Secret Manager, task đang chạy vẫn giữ giá trị lúc khởi động. Tạo deployment mới hoặc chọn **Force new deployment** sau khi lưu secret. Không sửa task bằng cách đổi mutable tag vì ECR repository đang immutable.

Frontend cập nhật riêng: build lại đúng biến Vite → aws s3 sync → CloudFront invalidation. Thay environment variable trên AWS không sửa JS đã build.

## 16. Chụp bằng chứng workshop

Chụp ngay sau khi từng phần hoàn tất; không đợi cuối buổi vì một số resource sẽ bị xóa khi cleanup. Tên ảnh dưới đây giữ nguyên để khớp cấu trúc workshop:

| **Thư mục/ảnh**                                        | **Nội dung cần chứng minh**                                                       |
| ------------------------------------------------------ | --------------------------------------------------------------------------------- |
| `02-prerequisites/aws-cli/aws-cli-and-validation.png`  | CLI trả đúng account/region và prefix list lookup thành công                      |
| `04-network/vpc/vpc-created.png`                       | VPC CIDR và DNS settings                                                          |
| `04-network/subnets-routes/private-route-tables.png`   | Bốn subnet, route private-a→NAT-A và private-b→NAT-B                              |
| `04-network/vpc-endpoints/endpoints-created.png`       | Một S3 Gateway Endpoint và hai NAT Gateway Available; không có Interface Endpoint |
| `05-secrets-manager/application-secret/key-names.png`  | Secret name và bốn tên key; che value                                             |
| `05-secrets-manager/iam/secret-read-policy.png`        | Execution role chỉ có quyền đọc secret được chỉ định                              |
| `06-ecr/image-push/repository-created.png`             | ECR private repository, immutable tag                                             |
| `06-ecr/image-push/image-pushed.png`                   | Tag mới và digest image                                                           |
| `07-ecs/cluster-service/service-running.png`           | Cluster/service, desired/running count                                            |
| `07-ecs/health/target-group-health.png`                | ECS task IP healthy                                                               |
| `08-s3/frontend-assets/frontend-assets.png`            | index.html và assets/ trong bucket riêng tư                                       |
| `09-cloudfront/distribution/distribution-config.png`   | S3 + ALB origins, /api/\* → ALB, default → S3                                     |
| `09-cloudfront/spa-routing/function-live.png`          | Function LIVE gắn ở Viewer request của default behavior                           |
| `09-cloudfront/invalidation/invalidation-complete.png` | Invalidation status Completed                                                     |
| `10-verification/browser-login/live-homepage.png`      | Website CloudFront và trang sau đăng nhập; che email nếu cần                      |
| `10-verification/webhook-flow/webhook-flow.png`        | Kịch bản webhook các bước hoàn tất                                                |
| `10-verification/cloudwatch-logs/api-logs.png`         | Log request test không có exception                                               |
| `11-cleanup/cleanup-complete.png`                      | Các resource tính phí đã xóa/dừng và giữ lại bằng chứng cần thiết                 |

Không chụp Secret value, database password, publishable key nếu không cần thiết, JWT, email cá nhân, hoặc nội dung khách hàng thật. Với publishable key, vẫn nên che trong báo cáo; key này được phép ở frontend nhưng không cần lộ trong ảnh.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/app
```

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/api/health
```

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/app?page=dashboard
```

**Kiểm thử end-to-end - giá trị 4**

```text
/ecs/nexusoms-demo-api
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open website

Trên AWS Console, thực hiện **Open website** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/1. Open website.png" title="Open website" >}}

### Bước 2 - Checking overall dashboard

Trên AWS Console, thực hiện **Checking overall dashboard** như hình dưới đây.

**Giá trị dùng ở bước này**

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/app?page=dashboard
```

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/2. Checking overall dashboard.png" title="Checking overall dashboard" >}}

### Bước 3 - Create product

Trên AWS Console, thực hiện **Create product** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/3. Create product.png" title="Create product" >}}

### Bước 4 - Checking order detail

Trên AWS Console, thực hiện **Checking order detail** như hình dưới đây.

**Giá trị dùng ở bước này**

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/app
```

**URL**

```text
https://<CLOUDFRONT_DOMAIN>/api/health
```

**Kiểm thử end-to-end - giá trị 4**

```text
/ecs/nexusoms-demo-api
```

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/4. Checking order detail.png" title="Checking order detail" >}}

### Bước 5 - Measuring product cost

Trên AWS Console, thực hiện **Measuring product cost** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/4.1. Measuring product cost.png" title="Measuring product cost" >}}

### Bước 6 - Using webhook to create order

Trên AWS Console, thực hiện **Using webhook to create order** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.5-Verification/end-to-end-verification/5. Using webhook to create order.png" title="Using webhook to create order" >}}

---
title: "Kiểm thử end-to-end"
date: 2026-11-15
weight: 1
chapter: false
---

## 1. Kiểm tra AWS

Trước khi mở website, xác nhận:

```text
ECS service: Running 1/1
Target group: Healthy
CloudFront distribution: Deployed
CloudFront invalidation: Completed
SQS main queue: Available 0, In flight 0
SQS dead-letter queue: Available 0
```

Nếu dùng cấu hình hai task theo sơ đồ, kiểm tra `Running 2/2`, mỗi AZ có một task và cả hai target đều `Healthy`.

## 2. Đăng nhập

Mở:

```text
https://<CLOUDFRONT_DOMAIN>
```

Đăng nhập bằng tài khoản đã tạo trong Supabase.

Mở DevTools → Network và kiểm tra:

```text
Chưa đăng nhập: /api/v1/session/me trả 401
Đăng nhập hợp lệ: /api/v1/session/me trả 200
API domain: <CLOUDFRONT_DOMAIN>
```

![Mở website NexusOMS](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/1-open-website.png)

## 3. Kiểm tra dashboard

Xác nhận dashboard tải dữ liệu và request `/api/*` trả về thành công.

Mở trực tiếp `/login`, `/app`, sau đó nhấn F5. Các route frontend phải tải lại bình thường; response lỗi API không được đổi thành HTML 200.

![Kiểm tra dashboard](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/2-checking-overall-dashboard.png)

## 4. Kiểm tra sản phẩm và tồn kho

Tạo sản phẩm, chọn kho và nhập số lượng ban đầu.

Kiểm tra dữ liệu trong NexusOMS. Phiên bản demo chưa đẩy sản phẩm hoặc tồn kho lên gian hàng thật.

![Tạo sản phẩm](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/3-create-product.png)

## 5. Kiểm tra đơn hàng

Mở danh sách đơn hàng. Trong **DevTools → Network**, xác nhận `/api/v1/orders` trả `200` với dữ liệu JSON. `/health` và target `Healthy` không thay thế bước kiểm tra database này.

Tạo đơn hàng bằng sản phẩm vừa tạo. Mở chi tiết đơn và kiểm tra tồn kho đã cập nhật.

![Kiểm tra chi tiết đơn hàng](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-checking-order-detail.png)

Kiểm tra chi phí và doanh thu của sản phẩm.

![Kiểm tra chi phí sản phẩm](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-1-measuring-product-cost.png)

## 6. Kiểm tra webhook và hủy đơn

Các bước dưới dùng simulator tích hợp trong ứng dụng để chủ động tạo sự kiện. Không cần đặt hoặc hủy đơn trên sàn thật.

Mở **Webhook Simulator → Vòng đời đơn**. Gửi webhook tạo đơn và chờ trạng thái **Đã hoàn tất**.

![Tạo đơn hàng bằng webhook](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/5-using-webhook-to-create-order.png)

Gửi lại cùng `eventId` và xác nhận hệ thống không tạo đơn thứ hai.

Mở tab **Hủy đơn**. Chọn đơn chưa xuất kho, nhập lý do rồi gửi webhook `order.cancelled`. Kiểm tra:

```text
Trạng thái đơn: Cancelled
Lý do hủy: đã được lưu
Tồn đang giữ: được nhả
Tồn khả dụng: được hoàn lại một lần
```

## 7. Kiểm tra tải cao qua SQS

1. Đăng nhập bằng tài khoản Manager hoặc Admin.
2. Mở **Webhook Simulator → Demo tải cao**.
3. Kiểm tra giao diện hiển thị chế độ `SQS`.
4. Chọn channel connection, warehouse và SKU mapping hợp lệ.
5. Bảo đảm tồn kho đủ cho tổng số lượng của batch.
6. Chọn batch `10` đơn và bắt đầu demo.
7. Mở **Amazon SQS → nexusoms-demo-webhook-queue.fifo → Monitoring**.
8. Theo dõi message available và in flight trong lúc worker xử lý.
9. Chờ batch hoàn tất; hai chỉ số trở về `0`.
10. Mở CloudWatch Logs và xác nhận không có lỗi `AccessDenied` với SQS.

Trong lúc batch chạy, xác nhận số message **Available** hoặc **In flight** tăng rồi trở về `0`. Đây là phần minh họa webhook được nhận nhanh và xử lý bất đồng bộ bởi worker.

Nếu message lỗi ba lần, kiểm tra `nexusoms-demo-webhook-dlq.fifo` và CloudWatch Logs trước khi redrive hoặc xóa message.

## 8. Kết thúc demo

Chọn **Kết thúc và dọn dữ liệu demo**. Xác nhận:

```text
Không còn đơn DEMO-*
Tồn kho trở về trạng thái trước khi chạy
Main queue không còn message
```

Nếu popup báo còn message trong DLQ, mở DLQ để xem lỗi. Chỉ xóa message sau khi đã kiểm tra CloudWatch Logs.

## 9. Kiểm tra CI/CD

Tạo branch, thay đổi nhỏ trên frontend và mở pull request. Sau khi CI thành công, merge vào `main`. Workflow phải cập nhật S3, chờ CloudFront invalidation hoàn tất và giữ ECS target ở trạng thái `Healthy`.

## 10. Xử lý lỗi nhanh

| Lỗi | Cách kiểm tra |
|---|---|
| `The config profile could not be found` | Chạy lại `aws login --profile nexusoms-demo --region ap-southeast-1`, sau đó kiểm tra bằng `aws sts get-caller-identity --profile nexusoms-demo` |
| Docker báo `error getting credentials` | Dùng Docker config tạm ở bước push image backend |
| ECS báo `secretsmanager:GetSecretValue AccessDenied` | Kiểm tra policy của execution role, đăng ký task definition revision mới và cập nhật service |
| Target group trả `500` | Mở CloudWatch Logs và kiểm tra lại secret bằng đoạn PowerShell ở phần Secrets Manager |
| CloudFront báo `globPattern invalid` | Xóa nội dung Path pattern và gõ lại `/api/*` |
| Route SPA trả `403` hoặc `404` | Build, Save và Publish CloudFront Function; kiểm tra association ở Default behavior |
| `NoSuchDistribution` | Dùng Distribution ID, không dùng domain CloudFront |
| GitHub Actions không assume được role | So sánh OIDC subject thực tế với trust policy của role |
| Hai workflow CD cùng hoạt động | Xóa `.github/workflows/cd.yml` và chỉ giữ `deploy-aws.yml` làm workflow deploy AWS |
| PR sửa workflow nhưng CI không chạy | Thêm `.github/workflows/**` vào path filter của cả backend CI và frontend CI; giữ `workflow_call` |

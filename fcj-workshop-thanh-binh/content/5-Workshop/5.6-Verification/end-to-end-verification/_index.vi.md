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
```

## 2. Đăng nhập

Mở:

```text
https://<CLOUDFRONT_DOMAIN>
```

Đăng nhập bằng tài khoản đã tạo trong Supabase.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/1-open-website.png" title="Mở website NexusOMS" >}}

## 3. Kiểm tra dashboard

Xác nhận dashboard tải dữ liệu và request `/api/*` trả về thành công.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/2-checking-overall-dashboard.png" title="Kiểm tra dashboard" >}}

## 4. Kiểm tra sản phẩm và tồn kho

Tạo sản phẩm, chọn kho và nhập số lượng ban đầu.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/3-create-product.png" title="Tạo sản phẩm" >}}

## 5. Kiểm tra đơn hàng

Tạo đơn hàng bằng sản phẩm vừa tạo. Mở chi tiết đơn và kiểm tra tồn kho đã cập nhật.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-checking-order-detail.png" title="Kiểm tra chi tiết đơn hàng" >}}

Kiểm tra chi phí và doanh thu của sản phẩm.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-1-measuring-product-cost.png" title="Kiểm tra chi phí sản phẩm" >}}

## 6. Kiểm tra webhook

Gửi webhook demo và xác nhận đơn hàng được tạo một lần.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/5-using-webhook-to-create-order.png" title="Tạo đơn hàng bằng webhook" >}}

## 7. Kiểm tra CI/CD

Thay đổi nhỏ trên frontend, commit và push lên `main`. Workflow phải cập nhật S3, tạo CloudFront invalidation và giữ ECS target ở trạng thái `Healthy`.

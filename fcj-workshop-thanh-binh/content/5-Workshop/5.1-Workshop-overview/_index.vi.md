---
title: "Tổng quan workshop"
date: 2026-11-15
weight: 1
chapter: false
---

NexusOMS sử dụng CloudFront làm điểm truy cập công khai, S3 lưu frontend React và ALB chuyển tiếp `/api/*` đến ECS Fargate trong private subnet. Mỗi task chạy API và background worker trong cùng ứng dụng .NET. API gửi sự kiện vào SQS FIFO; worker poll queue, xử lý và xóa message khi thành công. Message lỗi được chuyển sang dead-letter queue theo redrive policy.

Webhook Simulator tích hợp trong ứng dụng tạo các tình huống đặt đơn, hủy đơn và tải cao. Demo kiểm tra đơn hàng và tồn kho trong NexusOMS; chưa kết nối API gian hàng thật. Nhánh Open API trên sơ đồ thể hiện tích hợp mục tiêu.

Supabase cung cấp cả PostgreSQL và Authentication đã tích hợp trong ứng dụng. RDS PostgreSQL chỉ thay thế phần database; phần xác thực cần được xử lý riêng nếu chuyển toàn bộ Supabase.

GitHub Actions gửi OIDC token đến AWS STS để assume IAM role và nhận credential tạm thời. Workflow dùng credential này để đẩy backend image lên ECR, cập nhật ECS service, build frontend, đồng bộ lên S3 và tạo CloudFront invalidation.

![NexusOMS business flow architecture](/images/2-Proposal/noxus-aws-architect-1.png)

| Luồng | Nội dung |
| --- | --- |
| 5 | API gửi webhook hợp lệ vào SQS FIFO. |
| 6 | Worker poll SQS, xử lý và xóa message khi thành công. |
| 10 | Backend kết nối Supabase; nhánh Open API tới sàn là tích hợp mục tiêu. |
| 14 | Khi khởi động, Fargate tải ECR image layers từ S3 do AWS quản lý qua S3 Gateway Endpoint. |

Luồng 5–6 áp dụng cho cả hai bản sao API + Worker trong một ECS service. Luồng 14 bao gồm toàn tuyến Fargate → Endpoint → S3. ECR API/registry vẫn dùng NAT Gateway.

Để trình diễn hai task như sơ đồ, đặt `Desired tasks = 2` và xác nhận mỗi AZ có một task đang chạy. Bài lab mặc định dùng `1` task.


## Luồng triển khai

1. Chuẩn bị repository, Supabase và môi trường local.
2. Tạo VPC, subnet, route, NAT Gateway và security group.
3. Tạo secret, log group, IAM role, SQS FIFO, DLQ và audit logs.
4. Triển khai backend lên ECR, ALB và ECS Fargate.
5. Triển khai frontend lên S3 và CloudFront.
6. Kết nối GitHub Actions với AWS bằng OIDC.
7. Kiểm thử đơn hàng, hủy đơn và tải cao qua SQS.
8. Dọn dữ liệu demo rồi xóa tài nguyên AWS.

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

## Luồng hoạt động trên sơ đồ

| Luồng | Nội dung |
| ---: | --- |
| 1 | Người dùng truy cập hệ thống qua CloudFront. |
| 2 | CloudFront lấy giao diện React từ S3. |
| 3 | CloudFront chuyển request `/api/*` đến Application Load Balancer. |
| 4 | ALB phân phối request đến Fargate task đang healthy. |
| 5 | API gửi webhook hợp lệ vào SQS FIFO. |
| 6 | Worker poll SQS, xử lý theo thứ tự của message group và xóa message khi thành công. |
| 7 | Message lỗi vượt số lần nhận cho phép được chuyển sang dead-letter queue. |
| 8 | Fargate đi outbound qua NAT Gateway của Availability Zone tương ứng. |
| 9 | NAT Gateway chuyển lưu lượng outbound qua Internet Gateway. |
| 10 | Backend kết nối Supabase; nhánh Open API tới sàn là tích hợp mục tiêu. |
| 11 | GitHub Actions build và đẩy backend image lên ECR. |
| 12 | GitHub Actions đăng ký task definition revision mới và cập nhật ECS service. |
| 13 | ECS triển khai revision mới thành các Fargate task theo số lượng mong muốn. |
| 14 | Khi khởi động, Fargate tải ECR image layers từ S3 do AWS quản lý qua S3 Gateway Endpoint. |
| 15 | ALB ghi access log vào S3 audit bucket. |
| 16 | CloudTrail ghi management event vào S3 audit bucket. |

Đường nét đứt GitHub Actions → AWS STS thể hiện xác thực OIDC để nhận credential tạm thời cho CI/CD nên không đánh số riêng.

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

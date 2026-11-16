---
title: "Tổng quan workshop"
date: 2026-11-15
weight: 1
chapter: false
---

NexusOMS sử dụng CloudFront làm điểm truy cập công khai, S3 lưu frontend React và ALB chuyển tiếp `/api/*` đến ECS Fargate trong private subnet. API đưa webhook vào SQS FIFO; worker chạy trong ECS lấy message để xử lý. Message thất bại quá ba lần được chuyển sang dead-letter queue. Supabase cung cấp PostgreSQL và Authentication.

GitHub Actions kiểm tra source code, đẩy backend image lên ECR, cập nhật ECS service, build frontend, đồng bộ lên S3 và tạo CloudFront invalidation.

![NexusOMS business flow architecture](/images/2-Proposal/noxus-aws-architect-1.png)


## Luồng triển khai

1. Chuẩn bị repository, Supabase và môi trường local.
2. Tạo VPC, subnet, route, NAT Gateway và security group.
3. Tạo secret, log group, IAM role, SQS FIFO, DLQ và audit logs.
4. Triển khai backend lên ECR, ALB và ECS Fargate.
5. Triển khai frontend lên S3 và CloudFront.
6. Kết nối GitHub Actions với AWS bằng OIDC.
7. Kiểm thử đơn hàng, hủy đơn và tải cao qua SQS.
8. Dọn dữ liệu demo rồi xóa tài nguyên AWS.

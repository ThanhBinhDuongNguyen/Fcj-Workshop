---
title: "Tổng quan workshop"
date: 2026-11-15
weight: 1
chapter: false
---

NexusOMS sử dụng CloudFront làm điểm truy cập công khai, S3 lưu frontend React và ALB chuyển tiếp `/api/*` đến ECS Fargate trong private subnet. Supabase cung cấp PostgreSQL và Authentication.

GitHub Actions kiểm tra source code, đẩy backend image lên ECR, cập nhật ECS service, build frontend, đồng bộ lên S3 và tạo CloudFront invalidation.

{{< figure src="/images/5-Workshop/nexusoms/5.1-Workshop-overview/noxus-aws-architect-1.png" title="Kiến trúc NexusOMS trên AWS" >}}

## Luồng triển khai

1. Chuẩn bị repository, Supabase và môi trường local.
2. Tạo VPC, subnet, route, NAT Gateway và security group.
3. Tạo secret, log group, IAM role và audit logs.
4. Triển khai backend lên ECR, ALB và ECS Fargate.
5. Triển khai frontend lên S3 và CloudFront.
6. Kết nối GitHub Actions với AWS bằng OIDC.
7. Kiểm thử rồi dọn sạch tài nguyên.

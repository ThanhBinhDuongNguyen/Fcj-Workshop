---
title: "Security Group"
date: 2026-11-20
weight: 7
chapter: false
---

Tạo một Security Group cho ALB và một Security Group cho ECS task.

## Bước 1: Mở trang tạo Security Group

Mở **VPC → Security Groups → Create security group**.

![Mở trang tạo Security Group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/1-open-security-groups-dashboard-create-security-group.png)

## Bước 2: Tạo ALB Security Group

```text
Name: nexusoms-demo-alb-sg
VPC: nexusoms-demo-vpc
Inbound type: HTTP
Protocol: TCP
Port: 80
Source: <CLOUDFRONT_ORIGIN_PREFIX_LIST_ID>
```

Trong ô Source, chọn **Prefix list** rồi chọn `com.amazonaws.global.cloudfront.origin-facing`. Prefix list này đại diện cho mạng origin-facing của CloudFront.

![Cấu hình ALB Security Group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/2-full-basic-details-of-security-group.png)

## Bước 3: Tạo ECS task Security Group

```text
Name: nexusoms-demo-task-sg
VPC: nexusoms-demo-vpc
Inbound type: Custom TCP
Port: 8080
Source: nexusoms-demo-alb-sg
```

Không thêm inbound public cho ECS task.

![Cấu hình ECS task Security Group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/3-configure-the-second-security-group.png)

## Bước 4: Kiểm tra

- ALB chỉ nhận HTTP:80 từ CloudFront managed prefix list.
- ECS task chỉ nhận TCP:8080 từ `nexusoms-demo-alb-sg`.
- Giữ outbound mặc định cho bài lab.

Do ALB chỉ nhận traffic từ CloudFront origin-facing network, mở trực tiếp ALB DNS trên trình duyệt có thể timeout. Kiểm tra API bằng `https://<CLOUDFRONT_DOMAIN>/api/...`; không mở rộng Security Group hoặc đổi health check để che lỗi `500`.

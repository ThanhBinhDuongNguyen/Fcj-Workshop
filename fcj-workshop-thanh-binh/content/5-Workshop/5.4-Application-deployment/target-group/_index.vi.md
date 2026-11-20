---
title: "Target Group"
date: 2026-11-20
weight: 3
chapter: false
---

Tạo target group kiểu IP cho ECS Fargate.

## Bước 1: Tạo target group

Mở **EC2 → Target Groups → Create target group**.

![Mở trang tạo target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/1-open-target-groups-dashboard-create-target-group.png)

## Bước 2: Cấu hình target group

```text
Target type: IP addresses
Target group name: nexusoms-demo-api-tg
Protocol: HTTP
Port: 8080
IP address type: IPv4
VPC: nexusoms-demo-vpc
```

![Cấu hình target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/2-configure-target-group.png)

## Bước 3: Cấu hình health check

```text
Health check protocol: HTTP
Health check path: /health
Success codes: 200
```

![Cấu hình health check](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/3-choose-vpc-set-up-health-checks.png)

## Bước 4: Hoàn tất

Chọn **Next**. Không đăng ký IP thủ công. Chọn **Create target group**.

![Tạo target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/4-complete-create-target-group.png)

Kết quả cần đạt: target group sử dụng `IP addresses`, port `8080` và `/health` trả `200` sau khi ECS Service chạy.

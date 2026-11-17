---
title: "ECS Service"
date: 2026-11-13
weight: 8
chapter: false
---

Tạo ECS Service chạy task Fargate trong hai private subnet và đăng ký task với ALB target group.

## Bước 1: Tạo service

Mở **Amazon ECS → Clusters → nexusoms-demo-cluster → Services → Create**.

![Mở trang tạo ECS Service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/1-choose-service-tab-create.png)

## Bước 2: Cấu hình service

```text
Compute options: Launch type
Launch type: Fargate
Task definition family: nexusoms-demo-api
Service name: nexusoms-demo-api-service
Desired tasks: 1
```

Chọn revision task definition mới nhất. Khi cần trình diễn hai task, tăng **Desired tasks** lên `2`.

![Cấu hình ECS Service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/2-configure-service-detail.png)

## Bước 3: Cấu hình mạng

```text
VPC: nexusoms-demo-vpc
Subnets: private-a và private-b
Security Group: nexusoms-demo-task-sg
Auto-assign public IP: Disabled
```

![Cấu hình mạng cho service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/3-configure-network-for-service.png)

## Bước 4: Kết nối Load Balancer

```text
Load balancer type: Application Load Balancer
Load balancer: nexusoms-demo-alb
Listener: HTTP:80
Target group: nexusoms-demo-api-tg
Container: api
Container port: 8080
```

![Chọn Application Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4-configure-load-balancer-for-service.png)

![Chọn listener và target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4-1-configure-load-balancer-for-service.png)

## Bước 5: Hoàn tất

Chọn **Create** và chờ deployment ổn định.

![Tạo ECS Service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/5-click-create.png)

Kiểm tra:

- **Running tasks** bằng **Desired tasks**.
- Target trong `nexusoms-demo-api-tg` có trạng thái **Healthy**.
- CloudWatch log group `/ecs/nexusoms-demo-api` có log stream mới.
- Task không có public IP.

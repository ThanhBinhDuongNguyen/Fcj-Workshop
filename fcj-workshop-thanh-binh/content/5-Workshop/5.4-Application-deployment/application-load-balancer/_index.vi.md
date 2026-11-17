---
title: "Application Load Balancer"
date: 2026-11-13
weight: 4
chapter: false
---

Tạo Application Load Balancer để chuyển request API từ CloudFront đến các task Fargate.

## Bước 1: Mở trang tạo Load Balancer

Mở **EC2 → Load Balancers → Create load balancer**.

![Mở trang tạo Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/1-open-load-balancers-dashboard-create-load-balancer.png)

Chọn **Application Load Balancer → Create**.

![Chọn Application Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/2-choose-create-application-load-balancer.png)

## Bước 2: Cấu hình thông tin cơ bản

```text
Name: nexusoms-demo-alb
Scheme: Internet-facing
IP address type: IPv4
```

![Cấu hình thông tin cơ bản](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/3-configure-basic-load-balancer.png)

## Bước 3: Chọn VPC và subnet

```text
VPC: nexusoms-demo-vpc
Mappings: public-a và public-b
```

![Chọn VPC và hai public subnet](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/4-configure-network-mapping-for-load-balancer.png)

## Bước 4: Chọn Security Group

Chỉ chọn Security Group `nexusoms-demo-alb-sg`.

![Chọn Security Group của ALB](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/5-configure-security-group-for-load-balancer.png)

## Bước 5: Cấu hình listener và target group

```text
Protocol: HTTP
Port: 80
Default action: Forward to nexusoms-demo-api-tg
```

![Chọn target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/6-configure-target-group-for-load-balancer.png)

Chọn **Create load balancer** và chờ trạng thái chuyển thành **Active**.

## Bước 6: Ghi lại thông tin ALB

Mở ALB vừa tạo và ghi lại **DNS name** cùng **Load balancer ARN**.

![Sao chép ARN của ALB](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/7-copy-load-balancer-arn.png)

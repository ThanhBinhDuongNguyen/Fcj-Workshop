---
title: "Application Load Balancer"
date: 2026-11-13
weight: 4
chapter: false
---

ALB nằm trên hai public subnet và chuyển tiếp request từ CloudFront đến target group của ECS.

## 9.2. Target group và Application Load Balancer

Tạo Target Group trước:

1\. Mở **EC2 → Target Groups → Create target group**.

2\. Target type: **IP addresses**; protocol **HTTP**, port 8080.

3\. VPC nexusoms-demo-vpc; Target group name nexusoms-demo-api-tg.

4\. Health check protocol HTTP, path /health, success code 200; tạo target group. Không đăng ký IP thủ công - ECS tự đăng ký task IP.

Tạo ALB:

1\. EC2 → **Load Balancers → Create load balancer → Application Load Balancer**.

2\. Name nexusoms-demo-alb; Scheme **Internet-facing**; IP address type IPv4.

3\. Chọn VPC nexusoms-demo-vpc, mapping cả hai public subnet ở hai AZ.

4\. Security Group chỉ chọn nexusoms-demo-alb-sg.

5\. Listener HTTP:80, default action forward tới nexusoms-demo-api-tg; tạo ALB.

6\. Chờ ALB Active; copy DNS name trong Details.

Trong demo này, CloudFront → ALB dùng HTTP:80 để không cần domain/certificate riêng. Người dùng vẫn truy cập CloudFront bằng HTTPS. Khi có domain API riêng và ACM certificate đúng region, mới chuyển origin sang HTTPS.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Load Balancers → Create load balancer → Application Load Balancer**

```text
nexusoms-demo-alb
```

**Load Balancers → Create load balancer → Application Load Balancer**

```text
Internet-facing
```

**Internet-facing**

```text
IPv4
```

**Internet-facing**

```text
HTTP:80
```

**HTTP**

```text
nexusoms-demo-api-tg
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Load Balancers dashboard + Create load balancer

Thực hiện **Open Load Balancers dashboard + Create load balancer**.

**Giá trị dùng ở bước này**

**Load Balancers → Create load balancer → Application Load Balancer**

```text
nexusoms-demo-alb
```

**Load Balancers → Create load balancer → Application Load Balancer**

```text
Internet-facing
```

![Open Load Balancers dashboard + Create load balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/1-open-load-balancers-dashboard-create-load-balancer.png)

### Bước 2 - Choose create Application Load Balancer

Thực hiện **Choose create Application Load Balancer**.

![Choose create Application Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/2-choose-create-application-load-balancer.png)

### Bước 3 - Configure basic Load Balancer

Thực hiện **Configure basic Load Balancer**.

**Giá trị dùng ở bước này**

**Internet-facing**

```text
IPv4
```

**Internet-facing**

```text
HTTP:80
```

**HTTP**

```text
nexusoms-demo-api-tg
```

![Configure basic Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/3-configure-basic-load-balancer.png)

### Bước 4 - Configure Network mapping for Load Balancer

Thực hiện **Configure Network mapping for Load Balancer**.


![Configure Network mapping for Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/4-configure-network-mapping-for-load-balancer.png)

### Bước 5 - Configure Security group for Load Balancer

Thực hiện **Configure Security group for Load Balancer**.

![Configure Security group for Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/5-configure-security-group-for-load-balancer.png)

### Bước 6 - Configure Target group for Load Balancer

Thực hiện **Configure Target group for Load Balancer**.

![Configure Target group for Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/6-configure-target-group-for-load-balancer.png)

### Bước 7 - Copy Load balancer ARN

Thực hiện **Copy Load balancer ARN**.

![Copy Load balancer ARN](/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/7-copy-load-balancer-arn.png)

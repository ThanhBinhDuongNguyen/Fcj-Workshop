---
title: "Application Load Balancer"
date: 2026-11-13
weight: 5
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

Trên AWS Console, thực hiện **Open Load Balancers dashboard + Create load balancer** như hình dưới đây.

**Giá trị dùng ở bước này**

**Load Balancers → Create load balancer → Application Load Balancer**

```text
nexusoms-demo-alb
```

**Load Balancers → Create load balancer → Application Load Balancer**

```text
Internet-facing
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/1. Open Load Balancers dashboard + Create load balancer.png" title="Open Load Balancers dashboard + Create load balancer" >}}

### Bước 2 - Choose create Application Load Balancer

Trên AWS Console, thực hiện **Choose create Application Load Balancer** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/2. Choose create Application Load Balancer.png" title="Choose create Application Load Balancer" >}}

### Bước 3 - Configure basic Load Balancer

Trên AWS Console, thực hiện **Configure basic Load Balancer** như hình dưới đây.

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

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/3. Configure basic Load Balancer.png" title="Configure basic Load Balancer" >}}

### Bước 4 - Configure Network mapping for Load Balancer

Trên AWS Console, thực hiện **Configure Network mapping for Load Balancer** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/4. Configure Network mapping for Load Balancer.png" title="Configure Network mapping for Load Balancer" >}}

### Bước 5 - Configure Security group for Load Balancer

Trên AWS Console, thực hiện **Configure Security group for Load Balancer** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/5. Configure Security group for Load Balancer.png" title="Configure Security group for Load Balancer" >}}

### Bước 6 - Configure Target group for Load Balancer

Trên AWS Console, thực hiện **Configure Target group for Load Balancer** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/6. Configure Target group for Load Balancer.png" title="Configure Target group for Load Balancer" >}}

### Bước 7 - Copy Load balancer ARN

Trên AWS Console, thực hiện **Copy Load balancer ARN** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/application-load-balancer/7. Copy Load balancer ARN.png" title="Copy Load balancer ARN" >}}

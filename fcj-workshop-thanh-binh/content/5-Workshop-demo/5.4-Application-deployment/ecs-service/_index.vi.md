---
title: "ECS Service"
date: 2026-11-13
weight: 7
chapter: false
---

Chạy một task Fargate trong hai private subnet, không gán public IP và đăng ký service với ALB target group.

## 9.4. Tạo ECS service

1\. Cluster nexusoms-demo-cluster → **Services → Create**.

2\. Compute: Fargate; chọn task definition family/revision vừa tạo.

3\. Service name: nexusoms-demo-api-service (nếu tên này đã dùng, giữ tên service hiện có, không tạo service thứ hai).

4\. Desired tasks: dùng 1 để giảm chi phí khi thử; tăng lên 2 khi cần trình diễn task ở cả hai AZ. Desired 1 có nghĩa service chưa bảo đảm task luôn nằm ở cả hai AZ.

5\. Networking: VPC nexusoms-demo-vpc, Subnets chọn cả private-a và private-b; SG nexusoms-demo-task-sg; **Auto-assign public IP = DISABLED**.

6\. Load balancing: Application Load Balancer; chọn nexusoms-demo-alb, listener HTTP:80, target group nexusoms-demo-api-tg; container api, port 8080.

7\. Tạo service, chờ deployment ổn định.

**Kiểm tra trước khi tiếp tục:** ECS Service có Running tasks bằng Desired count; Target Group → Targets có IP của task và trạng thái **Healthy**; CloudWatch log group có stream mới với Application started và Now listening on: http://\[::\]:8080 hoặc địa chỉ cổng 8080. Task không có public IP.

Không test /health bằng https://&lt;CLOUDFRONT_DOMAIN&gt;/health: default CloudFront behavior trỏ S3 nên path này không phải API health route. ALB SG cũng chặn truy cập laptop trực tiếp theo thiết kế; dùng Target Group health, ECS events và CloudWatch logs để kiểm tra.

Ảnh cần chụp: 07-ecs/cluster-service/service-running.png và 07-ecs/health/target-group-health.png.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Services → Create**

```text
nexusoms-demo-api-service
```

**Desired tasks**

```text
Desired tasks: 1
```

**Auto-assign public IP**

```text
Auto-assign public IP: Disabled
```

**Container**

```text
Container: api
```

**Container port**

```text
Container port: 8080
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Choose service tab + Create

Trên AWS Console, thực hiện **Choose service tab + Create** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/1. Choose service tab + Create.png" title="Choose service tab + Create" >}}

### Bước 2 - Configure service detail

Trên AWS Console, thực hiện **Configure service detail** như hình dưới đây.

**Giá trị dùng ở bước này**

**Services → Create**

```text
nexusoms-demo-api-service
```

**Desired tasks**

```text
Desired tasks: 1
```

**Auto-assign public IP**

```text
Auto-assign public IP: Disabled
```

**Container**

```text
Container: api
```

**Container port**

```text
Container port: 8080
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/2. Configure service detail.png" title="Configure service detail" >}}

### Bước 3 - Configure Network for service

Trên AWS Console, thực hiện **Configure Network for service** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/3. Configure Network for service.png" title="Configure Network for service" >}}

### Bước 4 - Configure Load balancer for service

Trên AWS Console, thực hiện **Configure Load balancer for service** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4. Configure Load balancer for service.png" title="Configure Load balancer for service" >}}

### Bước 5 - Configure Load balancer for service

Trên AWS Console, thực hiện **Configure Load balancer for service** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4.1. Configure Load balancer for service.png" title="Configure Load balancer for service" >}}

### Bước 6 - Click create

Trên AWS Console, thực hiện **Click create** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/5. Click create.png" title="Click create" >}}

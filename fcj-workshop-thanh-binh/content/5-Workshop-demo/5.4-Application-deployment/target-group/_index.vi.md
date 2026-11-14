---
title: "Target Group"
date: 2026-11-13
weight: 4
chapter: false
---

Tạo target group kiểu **IP addresses** vì Fargate dùng `awsvpc`. Chọn VPC `nexusoms-demo-vpc`, cổng `8080` và health check `/health`; không đăng ký IP thủ công.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Target Group - giá trị 1**

```text
nexusoms-demo-api-tg
```

**Target type**

```text
Target type: IP addresses
```

**Protocol**

```text
Protocol: HTTP
```

**Port**

```text
Port: 8080
```

**Health check path**

```text
Health check path: /health
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Target Groups dashboard + Create target group

Trên AWS Console, thực hiện **Open Target Groups dashboard + Create target group** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/1-open-target-groups-dashboard-create-target-group.png" title="Open Target Groups dashboard + Create target group" >}}

### Bước 2 - Configure target group

Trên AWS Console, thực hiện **Configure target group** như hình dưới đây.

**Giá trị dùng ở bước này**

**Target Group - giá trị 1**

```text
nexusoms-demo-api-tg
```

**Target type**

```text
Target type: IP addresses
```

**Protocol**

```text
Protocol: HTTP
```

**Port**

```text
Port: 8080
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/2-configure-target-group.png" title="Configure target group" >}}

### Bước 3 - Choose VPC + Set up health checks

Trên AWS Console, thực hiện **Choose VPC + Set up health checks** như hình dưới đây.

**Giá trị dùng ở bước này**

**Health check path**

```text
Health check path: /health
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/3-choose-vpc-set-up-health-checks.png" title="Choose VPC + Set up health checks" >}}

### Bước 4 - Complete create target group

Trên AWS Console, thực hiện **Complete create target group** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/target-group/4-complete-create-target-group.png" title="Complete create target group" >}}

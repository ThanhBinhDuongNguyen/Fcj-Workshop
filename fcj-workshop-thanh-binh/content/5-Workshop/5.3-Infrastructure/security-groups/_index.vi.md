---
title: "Security Group"
date: 2026-11-13
weight: 7
chapter: false
---

ALB chỉ nhận HTTP từ CloudFront origin-facing prefix list; ECS chỉ nhận cổng 8080 từ security group của ALB.

## 6.1. Tạo hai Security Group

Mở **VPC → Security Groups → Create security group**. Mỗi SG phải ở VPC nexusoms-demo-vpc.

| **Security Group**      | **Inbound rule** | **Source phải chọn**                | **Dùng cho**                                |
| ----------------------- | ---------------- | ----------------------------------- | ------------------------------------------- |
| `nexusoms-demo-alb-sg`  | HTTP TCP 80      | Prefix list ID pl-... ở phần 2      | ALB chỉ nhận request đến từ mạng CloudFront |
| `nexusoms-demo-task-sg` | Custom TCP 8080  | Security Group nexusoms-demo-alb-sg | ECS chỉ nhận request từ ALB                 |

Trong ô Source của ALB SG, chọn **Prefix lists** rồi tìm/chọn prefix list com.amazonaws.global.cloudfront.origin-facing theo ID đã tra. Không chọn **Anywhere-IPv4 / 0.0.0.0/0**. Outbound có thể giữ mặc định cho demo. Không thêm inbound public cho ECS task.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**CloudFront value**

```text
nexusoms-demo-alb-sg | TCP 80 | <CLOUDFRONT_ORIGIN_PREFIX_LIST_ID>
```

**Task definition value**

```text
nexusoms-demo-task-sg | TCP 8080 | nexusoms-demo-alb-sg
```

## Thao tác trên AWS Console

### Bước 1 - Open Security groups dashboard + Create security group


![Open Security groups dashboard + Create security group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/1-open-security-groups-dashboard-create-security-group.png)

### Bước 2 - Full basic details of security group

**Giá trị dùng ở bước này**

**CloudFront value**

```text
nexusoms-demo-alb-sg | TCP 80 | <CLOUDFRONT_ORIGIN_PREFIX_LIST_ID>
```

**Task definition value**

```text
nexusoms-demo-task-sg | TCP 8080 | nexusoms-demo-alb-sg
```

![Full basic details of security group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/2-full-basic-details-of-security-group.png)

### Bước 3 - Configure the second security group

![Configure the second security group](/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/3-configure-the-second-security-group.png)

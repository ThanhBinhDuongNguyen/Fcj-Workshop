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

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Security groups dashboard + Create security group

Trên AWS Console, thực hiện **Open Security groups dashboard + Create security group** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/1. Open Security groups dashboard + Create security group.png" title="Open Security groups dashboard + Create security group" >}}

### Bước 2 - Full basic details of security group

Trên AWS Console, thực hiện **Full basic details of security group** như hình dưới đây.

**Giá trị dùng ở bước này**

**CloudFront value**

```text
nexusoms-demo-alb-sg | TCP 80 | <CLOUDFRONT_ORIGIN_PREFIX_LIST_ID>
```

**Task definition value**

```text
nexusoms-demo-task-sg | TCP 8080 | nexusoms-demo-alb-sg
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/2. Full basic details of security group.png" title="Full basic details of security group" >}}

### Bước 3 - Configure the second security group

Trên AWS Console, thực hiện **Configure the second security group** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/security-groups/3. Configure the second security group.png" title="Configure the second security group" >}}

---
title: "Tạo NAT Gateway"
date: 2026-11-13
weight: 5
chapter: false
---

Mỗi Availability Zone có một NAT Gateway riêng để task trong private subnet truy cập Supabase và webhook ngoài AWS.

## 5.2. Tạo hai Elastic IP và hai NAT Gateway

1\. Vào **VPC → Elastic IPs → Allocate Elastic IP address**.

2\. Chọn **Allocate** hai lần. Đặt Name tag khác nhau: nexusoms-demo-nat-eip-a và nexusoms-demo-nat-eip-b.

3\. Mở **VPC → NAT gateways → Create NAT gateway**.

4\. NAT A: Name nexusoms-demo-nat-a; Subnet nexusoms-demo-public-a; Connectivity type **Public**; chọn Elastic IP A; tạo.

5\. NAT B: Name nexusoms-demo-nat-b; Subnet nexusoms-demo-public-b; Connectivity type **Public**; chọn Elastic IP B; tạo.

6\. Chờ cả hai NAT Gateway có trạng thái **Available** trước khi tạo route private.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**NAT Gateway value**

```text
nexusoms-demo-nat-a | nexusoms-demo-public-a | nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-b | nexusoms-demo-public-b | nexusoms-demo-nat-eip-b
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open NAT Gateways dashboard + Create NAT Gateway

Trên AWS Console, thực hiện **Open NAT Gateways dashboard + Create NAT Gateway** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/1. Open NAT Gateways dashboard + Create NAT Gateway.png" title="Open NAT Gateways dashboard + Create NAT Gateway" >}}

### Bước 2 - Configure NAT Gateway settings

Trên AWS Console, thực hiện **Configure NAT Gateway settings** như hình dưới đây.

**Giá trị dùng ở bước này**

**NAT Gateway value**

```text
nexusoms-demo-nat-a | nexusoms-demo-public-a | nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-b | nexusoms-demo-public-b | nexusoms-demo-nat-eip-b
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/2. Configure NAT Gateway settings.png" title="Configure NAT Gateway settings" >}}

### Bước 3 - Click Create NAT Gateway

Trên AWS Console, thực hiện **Click Create NAT Gateway** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/3. Click Create NAT Gateway.png" title="Click Create NAT Gateway" >}}

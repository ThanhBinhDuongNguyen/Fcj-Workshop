---
title: "Tạo public và private subnet"
date: 2026-11-20
weight: 2
chapter: false
---

Tạo bốn subnet trên hai Availability Zone.

## Bước 1: Mở trang tạo subnet

Mở **VPC → Subnets → Create subnet**.

![Mở trang tạo subnet](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/1-open-subnets-dashboard-create-subnet.png)

Chọn VPC:

```text
nexusoms-demo-vpc
```

![Chọn VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/2-choose-vpc-id.png)

## Bước 2: Tạo public subnet A

```text
Name: nexusoms-demo-public-a
Availability Zone: AZ A
IPv4 CIDR: 10.80.0.0/24
```

![Cấu hình public subnet A](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/3-configure-public-subnet-a.png)

## Bước 3: Tạo public subnet B

```text
Name: nexusoms-demo-public-b
Availability Zone: AZ B
IPv4 CIDR: 10.80.1.0/24
```

![Cấu hình public subnet B](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/4-configure-public-subnet-b.png)

## Bước 4: Tạo private subnet A

```text
Name: nexusoms-demo-private-a
Availability Zone: cùng AZ với public-a
IPv4 CIDR: 10.80.10.0/24
```

![Cấu hình private subnet A](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/5-configure-private-subnet-a.png)

## Bước 5: Tạo private subnet B

```text
Name: nexusoms-demo-private-b
Availability Zone: cùng AZ với public-b
IPv4 CIDR: 10.80.11.0/24
```

![Cấu hình private subnet B](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/6-configure-private-subnet-b.png)

Chọn **Create subnet**.

## Bước 6: Bật public IPv4 cho public subnet

Chọn `nexusoms-demo-public-a` → **Actions → Edit subnet settings**.

![Mở subnet settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/7-edit-subnet-settings.png)

Bật **Enable auto-assign public IPv4 address** và lưu.

![Bật auto-assign public IPv4](/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/8-enable-auto-assign-public-ipv4-address.png)

Lặp lại cho `nexusoms-demo-public-b`. Giữ tùy chọn này tắt trên hai private subnet.

## Bước 7: Kiểm tra

| Subnet | CIDR | Public IPv4 |
| --- | --- | --- |
| `nexusoms-demo-public-a` | `10.80.0.0/24` | Bật |
| `nexusoms-demo-public-b` | `10.80.1.0/24` | Bật |
| `nexusoms-demo-private-a` | `10.80.10.0/24` | Tắt |
| `nexusoms-demo-private-b` | `10.80.11.0/24` | Tắt |

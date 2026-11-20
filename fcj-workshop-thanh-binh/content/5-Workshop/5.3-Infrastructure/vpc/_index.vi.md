---
title: "Tạo VPC"
date: 2026-11-20
weight: 1
chapter: false
---

Tạo VPC cho toàn bộ tài nguyên của workshop.

## Bước 1: Chọn Region và tạo budget

Chọn Region:

```text
Asia Pacific (Singapore)
ap-southeast-1
```

Mở **Billing and Cost Management → Budgets → Create budget**. Tạo Monthly cost budget với các ngưỡng cảnh báo `50%`, `80%` và `100%`.

## Bước 2: Mở trang tạo VPC

Mở **VPC → Your VPCs**.

![Mở Your VPCs](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/1-open-your-vpcs-dashboard.png)

Chọn **Create VPC**.

![Chọn Create VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/2-click-create-vpc.png)

## Bước 3: Cấu hình VPC

```text
Resources to create: VPC only
Name tag: nexusoms-demo-vpc
IPv4 CIDR: 10.80.0.0/16
IPv6 CIDR: No IPv6 CIDR block
Tenancy: Default
```

![Cấu hình VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/3-configure-vpc-settings.png)

Chọn **Create VPC**.

## Bước 4: Bật DNS

Chọn VPC vừa tạo → **Actions → Edit VPC settings**.

![Mở VPC settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/4-choose-action-edit-vpc-settings.png)

Bật:

```text
Enable DNS resolution
Enable DNS hostnames
```

![Bật DNS settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/5-check-2-box-in-dns-settings.png)

## Bước 5: Ghi lại VPC ID

Sao chép VPC ID để sử dụng ở các bước tiếp theo.

![Sao chép VPC ID](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/6-copy-vpc-id.png)

Kết quả cần đạt: CIDR là `10.80.0.0/16` và cả hai DNS setting đều bật.

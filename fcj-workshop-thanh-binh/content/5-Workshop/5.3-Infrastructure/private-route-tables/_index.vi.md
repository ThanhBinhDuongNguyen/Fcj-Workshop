---
title: "Private route table"
date: 2026-11-20
weight: 6
chapter: false
---

Tạo một route table riêng cho mỗi private subnet và định tuyến qua NAT Gateway cùng Availability Zone.

## Bước 1: Tạo route table A

Mở **VPC → Route tables → Create route table**.

![Mở trang tạo route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/1-open-route-tables-dashboard-click-create-route-table.png)

```text
Name: nexusoms-demo-private-a-rt
VPC: nexusoms-demo-vpc
```

![Tạo private route table A](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/2-fill-name-choose-vpc-create-route-table.png)

## Bước 2: Thêm route qua NAT A

Mở route table → **Routes → Edit routes**.

![Mở Edit routes](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/3-choose-routes-edit-routes.png)

```text
Destination: 0.0.0.0/0
Target: NAT Gateway → nexusoms-demo-nat-a
```

![Thêm route qua NAT A](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/4-add-route-configure-nat-gateway.png)

## Bước 3: Liên kết private subnet A

Chọn **Subnet associations → Edit subnet associations**.

![Mở subnet associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/5-choose-subnet-associations-edit-subnet-associations.png)

Chỉ chọn `nexusoms-demo-private-a`, sau đó lưu.

![Liên kết private subnet A](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/6-choose-subnet-private-a-save-associations.png)

## Bước 4: Tạo route table B

Lặp lại với:

```text
Name: nexusoms-demo-private-b-rt
Destination: 0.0.0.0/0
Target: NAT Gateway → nexusoms-demo-nat-b
Subnet association: nexusoms-demo-private-b
```

![Hoàn tất hai private route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/7-complete-create-private-route-table.png)

## Bước 5: Kiểm tra

- Private A đi qua NAT A.
- Private B đi qua NAT B.
- Không private route table nào trỏ trực tiếp đến Internet Gateway.

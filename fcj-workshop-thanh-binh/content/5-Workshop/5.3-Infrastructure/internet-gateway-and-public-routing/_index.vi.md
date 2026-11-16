---
title: "Internet Gateway và public routing"
date: 2026-11-13
weight: 3
chapter: false
---

Gắn Internet Gateway vào VPC và dùng một public route table chung cho hai public subnet.

## 5.1. Tạo Internet Gateway

### Bước 1: Mở trang Internet Gateway

Vào **VPC → Internet gateways**. Chọn **Create internet gateway**.

![Mở trang Internet Gateway và chọn Create internet gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/1-open-igw-dashboard-create-igw.png)

### Bước 2: Tạo Internet Gateway

Nhập **Name tag**:

```text
nexusoms-demo-igw
```

Chọn **Create internet gateway**.

![Nhập tên và tạo Internet Gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/2-put-name-tag-create-igw.png)

### Bước 3: Gắn Internet Gateway vào VPC

Chọn Internet Gateway vừa tạo. Chọn **Actions → Attach to a VPC**.

![Chọn Attach to a VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/3-attach-to-a-vpc.png)

### Bước 4: Chọn VPC

Chọn **nexusoms-demo-vpc**, sau đó chọn **Attach internet gateway**.

![Chọn VPC và gắn Internet Gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/4-choose-vpc-click-attach-igw.png)

## 5.2. Tạo public route table

### Bước 5: Mở trang Route tables

Vào **VPC → Route tables**. Chọn **Create route table**.

![Mở trang Route tables và chọn Create route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/5-open-route-tables-dashboard-create-route-table.png)

### Bước 6: Tạo route table

Nhập **Name**:

```text
nexusoms-demo-public-rt
```

Chọn **VPC: nexusoms-demo-vpc**, sau đó chọn **Create route table**.

![Nhập tên, chọn VPC và tạo route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/6-fill-name-choose-vpc-create-route-table.png)

### Bước 7: Mở phần chỉnh sửa route

Mở route table vừa tạo. Chọn **Routes → Edit routes**.

![Mở Routes và chọn Edit routes](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/7-choose-routes-edit-routes.png)

### Bước 8: Thêm route ra Internet

Chọn **Add route** và nhập:

```text
Destination: 0.0.0.0/0
Target: Internet Gateway → nexusoms-demo-igw
```

Chọn **Save changes**.

![Thêm route đến Internet Gateway và lưu thay đổi](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/8-add-route-choose-internet-gateway-save-changes.png)

## 5.3. Liên kết public subnet

### Bước 9: Mở Subnet associations

Chọn **Subnet associations → Edit subnet associations**.

![Mở Subnet associations và chọn Edit subnet associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/9-choose-subnet-associations-edit-subnet-associations.png)

### Bước 10: Chọn hai public subnet

Chọn hai subnet:

```text
nexusoms-demo-public-a
nexusoms-demo-public-b
```

Chọn **Save associations**.

![Chọn hai public subnet và lưu liên kết](/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/10-choose-2-public-subnet-save-associations.png)

## Kiểm tra kết quả

- Tab **Routes** có route `local` của VPC.
- Tab **Routes** có `0.0.0.0/0` trỏ đến `nexusoms-demo-igw`.
- Tab **Subnet associations** có `nexusoms-demo-public-a` và `nexusoms-demo-public-b`.

---
title: "Tạo NAT Gateway"
date: 2026-11-13
weight: 5
chapter: false
---

Tạo một NAT Gateway trong mỗi Availability Zone để task trong private subnet kết nối outbound tới Supabase và các dịch vụ AWS dùng public endpoint. Webhook Simulator chạy trong ứng dụng.

## 5.1. Tạo NAT Gateway A

### Bước 1: Mở trang NAT Gateway

Vào **VPC → NAT gateways**. Chọn **Create NAT gateway**.

![Mở trang NAT Gateway và chọn Create NAT gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/1-open-nat-gateways-dashboard-create-nat-gateway.png)

### Bước 2: Cấu hình NAT Gateway A

Nhập các giá trị:

```text
Name: nexusoms-demo-nat-a
Subnet: nexusoms-demo-public-a
Connectivity type: Public
Elastic IP allocation ID: <ALLOCATION_ID_A>
```

![Cấu hình NAT Gateway A](/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/2-configure-nat-gateway-settings.png)

### Bước 3: Tạo NAT Gateway A

Kiểm tra lại cấu hình, sau đó chọn **Create NAT gateway**.

![Tạo NAT Gateway A](/images/5-Workshop/nexusoms/5.3-Infrastructure/nat-gateways/3-click-create-nat-gateway.png)

## 5.2. Tạo NAT Gateway B

### Bước 4: Lặp lại thao tác cho NAT Gateway B

Chọn **Create NAT gateway** và nhập bộ giá trị thứ hai:

```text
Name: nexusoms-demo-nat-b
Subnet: nexusoms-demo-public-b
Connectivity type: Public
Elastic IP allocation ID: <ALLOCATION_ID_B>
```

Chọn **Create NAT gateway**.

## Kiểm tra kết quả

Chờ quá trình khởi tạo hoàn tất và kiểm tra:

- `nexusoms-demo-nat-a` có trạng thái **Available**.
- `nexusoms-demo-nat-b` có trạng thái **Available**.
- NAT A nằm trong `public-a` và dùng `<ALLOCATION_ID_A>`.
- NAT B nằm trong `public-b` và dùng `<ALLOCATION_ID_B>`.

Chỉ tiếp tục tạo private route table sau khi cả hai NAT Gateway chuyển sang **Available**.

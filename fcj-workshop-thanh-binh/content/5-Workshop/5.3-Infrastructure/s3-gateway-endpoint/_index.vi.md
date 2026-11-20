---
title: "S3 Gateway Endpoint"
date: 2026-11-20
weight: 8
chapter: false
---

Tạo một S3 Gateway Endpoint cho hai private route table.

## Bước 1: Tạo endpoint

Mở **VPC → Endpoints → Create endpoint**.

![Mở trang tạo endpoint](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/1-open-endpoints-dashboard-create-endpoint.png)

## Bước 2: Cấu hình thông tin cơ bản

```text
Name tag: nexusoms-demo-s3-endpoint
Service category: AWS services
```

![Cấu hình endpoint](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/2-fill-name-tag-choose-type.png)

## Bước 3: Chọn dịch vụ S3

Tìm và chọn:

```text
com.amazonaws.ap-southeast-1.s3
Type: Gateway
```

![Chọn S3 Gateway service](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/3-choose-s3-service.png)

## Bước 4: Chọn VPC và route table

```text
VPC: nexusoms-demo-vpc
Route tables: nexusoms-demo-private-a-rt
Route tables: nexusoms-demo-private-b-rt
Policy: Full access
```

Gateway Endpoint không sử dụng Security Group.

![Chọn hai private route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/4-choose-2-private-route-tables-full-access-for-policy.png)

## Bước 5: Hoàn tất

Chọn **Create endpoint** và chờ trạng thái **Available**.

![Tạo S3 Gateway Endpoint](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/5-click-create-endpoint.png)

Endpoint này chỉ phục vụ lưu lượng S3. Kết nối đến Supabase và API bên ngoài vẫn đi qua NAT Gateway.

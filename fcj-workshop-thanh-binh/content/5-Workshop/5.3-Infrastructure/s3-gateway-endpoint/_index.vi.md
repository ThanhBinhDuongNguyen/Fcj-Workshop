---
title: "S3 Gateway Endpoint"
date: 2026-11-13
weight: 8
chapter: false
---

Gateway Endpoint cho S3 không tính phí theo giờ và tránh đưa lưu lượng S3 của private subnet qua NAT Gateway.

## 6.2. Tạo một S3 Gateway Endpoint


**S3 Gateway Endpoint**

- Service category: AWS services; tìm com.amazonaws.ap-southeast-1.s3.
- VPC: nexusoms-demo-vpc.
- Endpoint type: Gateway.
- Route tables: chọn cả nexusoms-demo-private-a-rt và nexusoms-demo-private-b-rt.
- Security Group: không có; Gateway Endpoint không dùng SG.
- Policy: Full access cho demo; chọn Create endpoint.

Chờ S3 Gateway Endpoint ở trạng thái Available; hai NAT Gateway cũng phải Available trước khi tạo ECS service.


## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**S3 Gateway Endpoint**

```text
com.amazonaws.ap-southeast-1.s3
```

**S3 Gateway Endpoint**

```text
nexusoms-demo-private-a-rt
```

**Private subnet value**

```text
nexusoms-demo-private-b-rt
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Endpoints dashboard + Create endpoint

Thực hiện **Open Endpoints dashboard + Create endpoint**.

**Giá trị dùng ở bước này**

**S3 Gateway Endpoint**

```text
com.amazonaws.ap-southeast-1.s3
```

![Open Endpoints dashboard + Create endpoint](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/1-open-endpoints-dashboard-create-endpoint.png)

### Bước 2 - Fill name tag + Choose type

Thực hiện **Fill name tag + Choose type**.

![Fill name tag + Choose type](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/2-fill-name-tag-choose-type.png)

### Bước 3 - Choose S3 service

Thực hiện **Choose S3 service**.

![Choose S3 service](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/3-choose-s3-service.png)

### Bước 4 - Choose 2 private route tables + Full access for policy

Thực hiện **Choose 2 private route tables + Full access for policy**.

**Giá trị dùng ở bước này**

**S3 Gateway Endpoint**

```text
nexusoms-demo-private-a-rt
```

**Private subnet value**

```text
nexusoms-demo-private-b-rt
```

![Choose 2 private route tables + Full access for policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/4-choose-2-private-route-tables-full-access-for-policy.png)

### Bước 5 - Click create endpoint

Thực hiện **Click create endpoint**.

![Click create endpoint](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/5-click-create-endpoint.png)

---
title: "S3 Gateway Endpoint"
date: 2026-11-13
weight: 8
chapter: false
---

Gateway Endpoint cho S3 không tính phí theo giờ và tránh đưa lưu lượng S3 của private subnet qua NAT Gateway.

## 6.2. Tạo một S3 Gateway Endpoint

Vào VPC → Endpoints → Create endpoint. Chỉ tạo một S3 Gateway Endpoint; không tạo Interface Endpoint cho ecr.api, ecr.dkr, logs hoặc secretsmanager.

**S3 Gateway Endpoint**

- Service category: AWS services; tìm com.amazonaws.ap-southeast-1.s3.
- VPC: nexusoms-demo-vpc.
- Endpoint type: Gateway.
- Route tables: chọn cả nexusoms-demo-private-a-rt và nexusoms-demo-private-b-rt.
- Security Group: không có; Gateway Endpoint không dùng SG.
- Policy: Full access cho demo; chọn Create endpoint.

**Không tạo các Interface Endpoint ecr.api, ecr.dkr, logs và secretsmanager.**

- ECS task lấy image từ ECR, đọc cấu hình trong Secrets Manager và gửi log đến CloudWatch Logs qua NAT Gateway trong cùng AZ.
- Private subnet A phải đi NAT-A; private subnet B phải đi NAT-B. Cả hai NAT phải Available, có EIP, và public route table của NAT có route 0.0.0.0/0 tới IGW.
- Không cần endpoint security group hoặc cấu hình Private DNS vì bản này chỉ dùng S3 Gateway Endpoint.
- S3 Gateway Endpoint không cần Security Group; route table association là cấu hình cần thiết.
- Cách này tránh phí cố định của bốn Interface Endpoint nhưng dữ liệu ECR, Secrets Manager và CloudWatch Logs đi qua NAT, có thể phát sinh phí xử lý dữ liệu.
- Supabase và webhook cũng đi ra ngoài qua NAT Gateway.

Chờ S3 Gateway Endpoint ở trạng thái Available; hai NAT Gateway cũng phải Available trước khi tạo ECS service.

Ảnh cần chụp: 04-network/vpc-endpoints/endpoints-created.png với một S3 Gateway Endpoint và hai NAT Gateway Available; không có Interface Endpoint.

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

Trên AWS Console, thực hiện **Open Endpoints dashboard + Create endpoint** như hình dưới đây.

**Giá trị dùng ở bước này**

**S3 Gateway Endpoint**

```text
com.amazonaws.ap-southeast-1.s3
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/1-open-endpoints-dashboard-create-endpoint.png" title="Open Endpoints dashboard + Create endpoint" >}}

### Bước 2 - Fill name tag + Choose type

Trên AWS Console, thực hiện **Fill name tag + Choose type** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/2-fill-name-tag-choose-type.png" title="Fill name tag + Choose type" >}}

### Bước 3 - Choose S3 service

Trên AWS Console, thực hiện **Choose S3 service** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/3-choose-s3-service.png" title="Choose S3 service" >}}

### Bước 4 - Choose 2 private route tables + Full access for policy

Trên AWS Console, thực hiện **Choose 2 private route tables + Full access for policy** như hình dưới đây.

**Giá trị dùng ở bước này**

**S3 Gateway Endpoint**

```text
nexusoms-demo-private-a-rt
```

**Private subnet value**

```text
nexusoms-demo-private-b-rt
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/4-choose-2-private-route-tables-full-access-for-policy.png" title="Choose 2 private route tables + Full access for policy" >}}

### Bước 5 - Click create endpoint

Trên AWS Console, thực hiện **Click create endpoint** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/5-click-create-endpoint.png" title="Click create endpoint" >}}

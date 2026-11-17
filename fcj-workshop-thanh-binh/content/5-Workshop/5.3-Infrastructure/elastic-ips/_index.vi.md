---
title: "Cấp phát Elastic IP"
date: 2026-11-13
weight: 4
chapter: false
---

Cấp phát hai Elastic IP trong `ap-southeast-1`. Đặt Name tag riêng cho từng địa chỉ rồi gắn lần lượt vào NAT Gateway A và B.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-b
```

## Thao tác trên AWS Console

### Bước 1 - Open Elastic IPs + Allocate Elastic IP


![Open Elastic IPs + Allocate Elastic IP](/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/1-open-elastic-ips-allocate-elastic-ip.png)

### Bước 2 - Fill the name tag for Elastic IP + Allocate

**Giá trị dùng ở bước này**

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-b
```

![Fill the name tag for Elastic IP + Allocate](/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/2-fill-the-name-tag-for-elastic-ip-allocate.png)

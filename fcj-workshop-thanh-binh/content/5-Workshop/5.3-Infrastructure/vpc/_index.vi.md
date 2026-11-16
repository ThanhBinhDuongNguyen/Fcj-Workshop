---
title: "Tạo VPC"
date: 2026-11-13
weight: 1
chapter: false
---

Tạo vùng mạng riêng cho toàn bộ tài nguyên của workshop và bật cả DNS resolution lẫn DNS hostnames.

## 4.1. Chọn region và tạo cảnh báo chi phí

1\. Mở AWS Console, nhìn bộ chọn region ở góc trên bên phải.

2\. Chọn **Asia Pacific (Singapore) / ap-southeast-1** trước khi tạo resource.

3\. Mở **Billing and Cost Management → Budgets → Create budget**.

4\. Chọn **Cost budget**, kỳ **Monthly**, đặt mức tiền phù hợp với credit còn lại và nhập email nhận cảnh báo.

5\. Thêm cảnh báo ở các ngưỡng 50%, 80% và 100%, rồi tạo budget.

Budget chỉ gửi cảnh báo, không tự dừng resource. Hai NAT Gateway, ALB, ECS chạy liên tục, CloudWatch log và truyền dữ liệu có thể tính tiền. S3 Gateway Endpoint không có phí theo giờ.

## 4.2. Tạo VPC

1\. Mở **VPC → Your VPCs → Create VPC**.

2\. Chọn **VPC only**.

3\. Điền **Name tag** nexusoms-demo-vpc, **IPv4 CIDR** 10.80.0.0/16, **IPv6 CIDR** No IPv6 CIDR block, **Tenancy** Default.

4\. Chọn **Create VPC**.

5\. Chọn VPC vừa tạo → **Actions → Edit VPC settings**.

6\. Bật **Enable DNS resolution** và **Enable DNS hostnames**, lưu.

**Kiểm tra:** VPC có CIDR 10.80.0.0/16; DNS resolution và DNS hostnames đều bật. Copy VPC ID vào bảng thông tin.


## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Region**

```text
ap-southeast-1
```

**Name tag**

```text
nexusoms-demo-vpc
```

**IPv4 CIDR**

```text
10.80.0.0/16
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Your VPCs dashboard

Thực hiện **Open Your VPCs dashboard**.


![Open Your VPCs dashboard](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/1-open-your-vpcs-dashboard.png)

### Bước 2 - Click create VPC

Thực hiện **Click create VPC**.


![Click create VPC](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/2-click-create-vpc.png)

### Bước 3 - Configure VPC settings

Thực hiện **Configure VPC settings**.

**Giá trị dùng ở bước này**

**Region**

```text
ap-southeast-1
```

**Name tag**

```text
nexusoms-demo-vpc
```

**IPv4 CIDR**

```text
10.80.0.0/16
```

![Configure VPC settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/3-configure-vpc-settings.png)

### Bước 4 - Choose Action + Edit VPC Settings

Thực hiện **Choose Action + Edit VPC Settings**.

![Choose Action + Edit VPC Settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/4-choose-action-edit-vpc-settings.png)

### Bước 5 - Check 2 box in DNS settings

Thực hiện **Check 2 box in DNS settings**.

![Check 2 box in DNS settings](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/5-check-2-box-in-dns-settings.png)

### Bước 6 - Copy VPC ID

Thực hiện **Copy VPC ID**.

![Copy VPC ID](/images/5-Workshop/nexusoms/5.3-Infrastructure/vpc/6-copy-vpc-id.png)

---
title: "Internet Gateway và public routing"
date: 2026-11-13
weight: 3
chapter: false
---

Gắn Internet Gateway vào VPC và dùng một public route table chung cho cả hai public subnet.

## 5.1. Internet Gateway và route cho public subnet

1\. Vào **VPC → Internet gateways → Create internet gateway**.

2\. Name: nexusoms-demo-igw; chọn **Create internet gateway**.

3\. Chọn IGW → **Actions → Attach to a VPC** → chọn nexusoms-demo-vpc → **Attach**.

4\. Mở **VPC → Route tables → Create route table**.

5\. Name nexusoms-demo-public-rt, VPC nexusoms-demo-vpc; tạo.

6\. Mở route table → **Routes → Edit routes → Add route**. Destination 0.0.0.0/0; Target chọn **Internet Gateway** rồi chọn nexusoms-demo-igw; lưu.

7\. Mở **Subnet associations → Edit subnet associations**; chọn cả public-a và public-b; lưu.

**Kiểm tra:** tab Routes có local route của VPC và 0.0.0.0/0 → igw-...; tab Subnet associations có đúng hai public subnet.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**VPC → Internet gateways → Create internet gateway**

```text
nexusoms-demo-igw
```

**VPC → Route tables → Create route table**

```text
nexusoms-demo-public-rt
```

**Routes → Edit routes → Add route**

```text
0.0.0.0/0
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open IGW dashboard + Create IGW

Trên AWS Console, thực hiện **Open IGW dashboard + Create IGW** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/1. Open IGW dashboard + Create IGW.png" title="Open IGW dashboard + Create IGW" >}}

### Bước 2 - Put name tag + Create IGW

Trên AWS Console, thực hiện **Put name tag + Create IGW** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/2. Put name tag + Create IGW.png" title="Put name tag + Create IGW" >}}

### Bước 3 - Attach to a VPC

Trên AWS Console, thực hiện **Attach to a VPC** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/3. Attach to a VPC.png" title="Attach to a VPC" >}}

### Bước 4 - Choose VPC + Click Attach IGW

Trên AWS Console, thực hiện **Choose VPC + Click Attach IGW** như hình dưới đây.

**Giá trị dùng ở bước này**

**VPC → Internet gateways → Create internet gateway**

```text
nexusoms-demo-igw
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/4. Choose VPC + Click Attach IGW.png" title="Choose VPC + Click Attach IGW" >}}

### Bước 5 - Open Route tables dashboard + Create Route table

Trên AWS Console, thực hiện **Open Route tables dashboard + Create Route table** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/5. Open Route tables dashboard + Create Route table.png" title="Open Route tables dashboard + Create Route table" >}}

### Bước 6 - Fill Name + Choose VPC + Create route table

Trên AWS Console, thực hiện **Fill Name + Choose VPC + Create route table** như hình dưới đây.

**Giá trị dùng ở bước này**

**VPC → Route tables → Create route table**

```text
nexusoms-demo-public-rt
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/6. Fill Name + Choose VPC + Create route table.png" title="Fill Name + Choose VPC + Create route table" >}}

### Bước 7 - Choose Routes + Edit routes

Trên AWS Console, thực hiện **Choose Routes + Edit routes** như hình dưới đây.

**Giá trị dùng ở bước này**

**Routes → Edit routes → Add route**

```text
0.0.0.0/0
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/7. Choose Routes + Edit routes.png" title="Choose Routes + Edit routes" >}}

### Bước 8 - Add route + choose Internet gateway + Save changes

Trên AWS Console, thực hiện **Add route + choose Internet gateway + Save changes** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/8. Add route + choose Internet gateway + Save changes.png" title="Add route + choose Internet gateway + Save changes" >}}

### Bước 9 - Choose subnet associations + Edit subnet associations

Trên AWS Console, thực hiện **Choose subnet associations + Edit subnet associations** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/9. Choose subnet associations + Edit subnet associations.png" title="Choose subnet associations + Edit subnet associations" >}}

### Bước 10 - Choose 2 public subnet + Save associations

Trên AWS Console, thực hiện **Choose 2 public subnet + Save associations** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/internet-gateway-and-public-routing/10. Choose 2 public subnet + Save associations.png" title="Choose 2 public subnet + Save associations" >}}

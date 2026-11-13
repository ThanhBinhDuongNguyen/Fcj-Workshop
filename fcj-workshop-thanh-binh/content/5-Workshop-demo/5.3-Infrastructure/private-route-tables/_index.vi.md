---
title: "Private route table"
date: 2026-11-13
weight: 6
chapter: false
---

Tạo route table riêng cho từng private subnet và định tuyến ra NAT Gateway cùng Availability Zone.

## 5.3. Route riêng cho private subnet

1\. Tạo route table nexusoms-demo-private-a-rt trong VPC nexusoms-demo-vpc.

2\. Thêm route Destination 0.0.0.0/0, Target **NAT Gateway** nexusoms-demo-nat-a; associate chỉ với nexusoms-demo-private-a.

3\. Tạo route table nexusoms-demo-private-b-rt.

4\. Thêm route Destination 0.0.0.0/0, Target NAT nexusoms-demo-nat-b; associate chỉ với nexusoms-demo-private-b.

Không nhập IP Supabase vào route table. Route NAT cho phép task trong private subnet khởi tạo kết nối đi ra Internet; nó không mở kết nối từ Internet đi thẳng vào ECS.

**Kiểm tra:** private route A đi NAT A; private route B đi NAT B; không route private nào trỏ thẳng tới IGW.

Ảnh cần chụp: 04-network/subnets-routes/private-route-tables.png.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**NAT Gateway value**

```text
nexusoms-demo-private-a-rt | 0.0.0.0/0 -> nexusoms-demo-nat-a
```

**NAT Gateway value**

```text
nexusoms-demo-private-b-rt | 0.0.0.0/0 -> nexusoms-demo-nat-b
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Route tables dashboard + Click create route table

Trên AWS Console, thực hiện **Open Route tables dashboard + Click create route table** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/1. Open Route tables dashboard + Click create route table.png" title="Open Route tables dashboard + Click create route table" >}}

### Bước 2 - Fill Name + Choose VPC + Create route table

Trên AWS Console, thực hiện **Fill Name + Choose VPC + Create route table** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/2. Fill Name + Choose VPC + Create route table.png" title="Fill Name + Choose VPC + Create route table" >}}

### Bước 3 - Choose routes + Edit routes

Trên AWS Console, thực hiện **Choose routes + Edit routes** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/3. Choose routes + Edit routes.png" title="Choose routes + Edit routes" >}}

### Bước 4 - Add Route + Configure NAT Gateway

Trên AWS Console, thực hiện **Add Route + Configure NAT Gateway** như hình dưới đây.

**Giá trị dùng ở bước này**

**NAT Gateway value**

```text
nexusoms-demo-private-a-rt | 0.0.0.0/0 -> nexusoms-demo-nat-a
```

**NAT Gateway value**

```text
nexusoms-demo-private-b-rt | 0.0.0.0/0 -> nexusoms-demo-nat-b
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/4. Add Route + Configure NAT Gateway.png" title="Add Route + Configure NAT Gateway" >}}

### Bước 5 - Choose Subnet associations + Edit subnet associations

Trên AWS Console, thực hiện **Choose Subnet associations + Edit subnet associations** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/5. Choose Subnet associations + Edit subnet associations.png" title="Choose Subnet associations + Edit subnet associations" >}}

### Bước 6 - Choose subnet private a + Save associations

Trên AWS Console, thực hiện **Choose subnet private a + Save associations** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/6. Choose subnet private a + Save associations.png" title="Choose subnet private a + Save associations" >}}

### Bước 7 - Complete create private route table

Trên AWS Console, thực hiện **Complete create private route table** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/7. Complete create private route table.png" title="Complete create private route table" >}}

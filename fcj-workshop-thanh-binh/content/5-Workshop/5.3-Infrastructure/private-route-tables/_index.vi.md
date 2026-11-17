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

### Bước 1 - Open Route tables dashboard + Click create route table

![Open Route tables dashboard + Click create route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/1-open-route-tables-dashboard-click-create-route-table.png)

### Bước 2 - Fill Name + Choose VPC + Create route table

![Fill Name + Choose VPC + Create route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/2-fill-name-choose-vpc-create-route-table.png)

### Bước 3 - Choose routes + Edit routes

![Choose routes + Edit routes](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/3-choose-routes-edit-routes.png)

### Bước 4 - Add Route + Configure NAT Gateway

**Giá trị dùng ở bước này**

**NAT Gateway value**

```text
nexusoms-demo-private-a-rt | 0.0.0.0/0 -> nexusoms-demo-nat-a
```

**NAT Gateway value**

```text
nexusoms-demo-private-b-rt | 0.0.0.0/0 -> nexusoms-demo-nat-b
```

![Add Route + Configure NAT Gateway](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/4-add-route-configure-nat-gateway.png)

### Bước 5 - Choose Subnet associations + Edit subnet associations

![Choose Subnet associations + Edit subnet associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/5-choose-subnet-associations-edit-subnet-associations.png)

### Bước 6 - Choose subnet private a + Save associations

![Choose subnet private a + Save associations](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/6-choose-subnet-private-a-save-associations.png)

### Bước 7 - Complete create private route table

![Complete create private route table](/images/5-Workshop/nexusoms/5.3-Infrastructure/private-route-tables/7-complete-create-private-route-table.png)

---
title: "Tạo public và private subnet"
date: 2026-11-13
weight: 2
chapter: false
---

Tạo bốn subnet trên hai Availability Zone. Bật auto-assign public IPv4 chỉ cho hai public subnet.

## 4.3. Tạo hai public và hai private subnet

Mở **VPC → Subnets → Create subnet**. Với từng subnet, chọn VPC nexusoms-demo-vpc, tên, Availability Zone và IPv4 CIDR trong bảng:

| **Name**                  | **AZ**    | **CIDR**        | **Gán public IPv4 tự động** |
| ------------------------- | --------- | --------------- | --------------------------- |
| `nexusoms-demo-public-a`  | AZ A      | `10.80.0.0/24`  | Bật                         |
| `nexusoms-demo-public-b`  | AZ B      | `10.80.1.0/24`  | Bật                         |
| `nexusoms-demo-private-a` | Cùng AZ A | `10.80.10.0/24` | Tắt                         |
| `nexusoms-demo-private-b` | Cùng AZ B | `10.80.11.0/24` | Tắt                         |

Chọn AZ trong menu AWS Console; không tự gõ tên AZ chưa xác nhận. Tạo xong, chọn từng public subnet → **Actions → Edit subnet settings** → bật **Enable auto-assign public IPv4 address**. Private subnet để tắt.

Nếu Console có thuộc tính **Block public access** cho subnet, không cần bật thuộc tính đó để biến subnet thành private. ECS private được bảo vệ bằng cách không gán public IP, không có route trực tiếp tới IGW và dùng Security Group giới hạn inbound. Không gán public IP cho ECS task.

**Kiểm tra:** có đủ bốn subnet trong cùng VPC; hai subnet private không bật auto-assign public IPv4; mỗi AZ có một public và một private subnet.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Public subnet value**

```text
nexusoms-demo-public-a | 10.80.0.0/24
```

**Public subnet value**

```text
nexusoms-demo-public-b | 10.80.1.0/24
```

**Private subnet value**

```text
nexusoms-demo-private-a | 10.80.10.0/24
```

**Private subnet value**

```text
nexusoms-demo-private-b | 10.80.11.0/24
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open subnets dashboard + Create subnet

Trên AWS Console, thực hiện **Open subnets dashboard + Create subnet** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/1-open-subnets-dashboard-create-subnet.png" title="Open subnets dashboard + Create subnet" >}}

### Bước 2 - Choose VPC ID

Trên AWS Console, thực hiện **Choose VPC ID** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/2-choose-vpc-id.png" title="Choose VPC ID" >}}

### Bước 3 - Configure public subnet a

Trên AWS Console, thực hiện **Configure public subnet a** như hình dưới đây.

**Giá trị dùng ở bước này**

**Public subnet value**

```text
nexusoms-demo-public-a | 10.80.0.0/24
```

**Public subnet value**

```text
nexusoms-demo-public-b | 10.80.1.0/24
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/3-configure-public-subnet-a.png" title="Configure public subnet a" >}}

### Bước 4 - Configure public subnet b

Trên AWS Console, thực hiện **Configure public subnet b** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/4-configure-public-subnet-b.png" title="Configure public subnet b" >}}

### Bước 5 - Configure private subnet a

Trên AWS Console, thực hiện **Configure private subnet a** như hình dưới đây.

**Giá trị dùng ở bước này**

**Private subnet value**

```text
nexusoms-demo-private-a | 10.80.10.0/24
```

**Private subnet value**

```text
nexusoms-demo-private-b | 10.80.11.0/24
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/5-configure-private-subnet-a.png" title="Configure private subnet a" >}}

### Bước 6 - Configure private subnet b

Trên AWS Console, thực hiện **Configure private subnet b** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/6-configure-private-subnet-b.png" title="Configure private subnet b" >}}

### Bước 7 - Edit subnet settings

Trên AWS Console, thực hiện **Edit subnet settings** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/7-edit-subnet-settings.png" title="Edit subnet settings" >}}

### Bước 8 - Enable auto-assign public IPv4 address

Trên AWS Console, thực hiện **Enable auto-assign public IPv4 address** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/subnets/8-enable-auto-assign-public-ipv4-address.png" title="Enable auto-assign public IPv4 address" >}}

---
title: "Cấp phát Elastic IP"
date: 2026-11-13
weight: 4
chapter: false
---

Cấp phát hai Elastic IP trong `ap-southeast-1`. Ảnh minh họa lần đầu; lặp lại thao tác và đổi Name tag cho địa chỉ thứ hai. Hai địa chỉ này sẽ được gắn lần lượt cho NAT Gateway A và B.

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

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Elastic IPs + Allocate Elastic IP

Trên AWS Console, thực hiện **Open Elastic IPs + Allocate Elastic IP** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/1-open-elastic-ips-allocate-elastic-ip.png" title="Open Elastic IPs + Allocate Elastic IP" >}}

### Bước 2 - Fill the name tag for Elastic IP + Allocate

Trên AWS Console, thực hiện **Fill the name tag for Elastic IP + Allocate** như hình dưới đây.

**Giá trị dùng ở bước này**

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-a
```

**NAT Gateway value**

```text
nexusoms-demo-nat-eip-b
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/2-fill-the-name-tag-for-elastic-ip-allocate.png" title="Fill the name tag for Elastic IP + Allocate" >}}

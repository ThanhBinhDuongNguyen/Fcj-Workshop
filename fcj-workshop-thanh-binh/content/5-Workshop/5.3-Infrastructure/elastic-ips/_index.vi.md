---
title: "Cấp phát Elastic IP"
date: 2026-11-20
weight: 4
chapter: false
---

Cấp phát hai Elastic IP và ghi lại Allocation ID để chọn đúng địa chỉ khi tạo NAT Gateway.

## Bước 1: Cấp phát Elastic IP A

Mở **VPC → Elastic IPs → Allocate Elastic IP address**.

![Mở trang cấp phát Elastic IP](/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/1-open-elastic-ips-allocate-elastic-ip.png)

Giữ cấu hình mặc định, thêm tag rồi chọn **Allocate**:

```text
Key: Name
Value: nexusoms-demo-nat-eip-a
```

![Đặt Name tag cho Elastic IP](/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/2-fill-the-name-tag-for-elastic-ip-allocate.png)

## Bước 2: Cấp phát Elastic IP B

Lặp lại thao tác với tag:

```text
Key: Name
Value: nexusoms-demo-nat-eip-b
```

## Bước 3: Ghi lại thông tin

Ghi lại đúng từng dòng trong danh sách Elastic IP:

| Name | Public IPv4 | Allocation ID |
| --- | --- | --- |
| `nexusoms-demo-nat-eip-a` | `<EIP_A>` | `<ALLOCATION_ID_A>` |
| `nexusoms-demo-nat-eip-b` | `<EIP_B>` | `<ALLOCATION_ID_B>` |

Dropdown của NAT Gateway có thể chỉ hiển thị IP và Allocation ID. Sử dụng bảng này để chọn đúng địa chỉ.

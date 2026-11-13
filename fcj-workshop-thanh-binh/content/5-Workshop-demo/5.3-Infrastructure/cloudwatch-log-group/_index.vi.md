---
title: "CloudWatch Log Group"
date: 2026-11-13
weight: 11
chapter: false
---

Trong giao diện hiện tại, mở **CloudWatch → Logs Management → Create log group**. Đây là log group mà task definition sẽ sử dụng.

## 7.4. Tạo CloudWatch Log Group

Mở **CloudWatch → Logs Management → Create log group**. Nhập /ecs/nexusoms-demo-api, retention 14 days; tạo. Task definition sẽ ghi log vào đây.

Ảnh cần chụp: 05-secrets-manager/application-secret/key-names.png chỉ để lộ tên key, che toàn bộ value. Chụp role và policy ở 05-secrets-manager/iam/secret-read-policy.png, không để lộ credentials.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**CloudWatch → Logs Management → Create log group**

```text
/ecs/nexusoms-demo-api
```

**CloudWatch → Logs Management → Create log group**

```text
14 days
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Log Management dashboard + Create log group

Trên AWS Console, thực hiện **Open Log Management dashboard + Create log group** như hình dưới đây.

**Giá trị dùng ở bước này**

**CloudWatch → Logs Management → Create log group**

```text
/ecs/nexusoms-demo-api
```

**CloudWatch → Logs Management → Create log group**

```text
14 days
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudwatch-log-group/1. Open Log Management dashboard + Create log group.png" title="Open Log Management dashboard + Create log group" >}}

### Bước 2 - Fill name + retention setting

Trên AWS Console, thực hiện **Fill name + retention setting** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudwatch-log-group/2. Fill name + retention setting.png" title="Fill name + retention setting" >}}

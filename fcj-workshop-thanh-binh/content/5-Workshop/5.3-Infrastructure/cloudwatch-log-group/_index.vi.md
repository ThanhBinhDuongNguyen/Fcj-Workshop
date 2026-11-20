---
title: "CloudWatch Log Group"
date: 2026-11-20
weight: 10
chapter: false
---

Tạo log group trước khi đăng ký ECS Task Definition.

## Bước 1: Tạo log group

Mở **CloudWatch → Logs → Log groups → Create log group**.

```text
Log group name: /ecs/nexusoms-demo-api
```

![Mở trang tạo log group](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudwatch-log-group/1-open-log-management-dashboard-create-log-group.png)

## Bước 2: Đặt retention

Mở log group vừa tạo và đặt:

```text
Retention setting: 14 days
```

![Đặt retention cho log group](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudwatch-log-group/2-fill-name-retention-setting.png)

Kết quả cần đạt: log group `/ecs/nexusoms-demo-api` tồn tại trước khi chạy ECS task.

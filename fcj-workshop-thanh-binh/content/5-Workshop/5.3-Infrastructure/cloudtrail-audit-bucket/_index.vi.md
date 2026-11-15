---
title: "CloudTrail và S3 audit bucket"
date: 2026-11-15
weight: 12
chapter: false
---

## 1. Tạo trail

Mở CloudTrail → **Trails → Create trail**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/1-open-cloudtrail-and-create-trail.png" title="Mở CloudTrail và tạo trail" >}}

Nhập:

```text
Trail name: nexusoms-demo-trail
Storage location: Create new S3 bucket
Log file SSE-KMS encryption: Disabled
Log file validation: Enabled
CloudWatch Logs: Disabled
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/2-configure-trail.png" title="Cấu hình trail" >}}

Trong **Choose log events**, chọn:

```text
Event type: Management events
API activity: Read và Write
```

Giữ các mục còn lại ở trạng thái tắt.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/3-configure-management-events.png" title="Chọn management events" >}}

Chọn **Create trail**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/4-create-trail.png" title="Tạo trail" >}}

## 2. Kiểm tra audit bucket

Mở trail, ghi lại tên S3 bucket được CloudTrail tạo rồi mở bucket.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/5-open-audit-bucket.png" title="Mở audit bucket" >}}

Trong **Permissions**, xác nhận **Block all public access** đang bật.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/6-check-block-public-access.png" title="Kiểm tra Block Public Access" >}}

Trong **Properties**, xác nhận default encryption là **SSE-S3**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/7-check-default-encryption.png" title="Kiểm tra encryption" >}}

Sau vài phút, bucket phải có prefix:

```text
AWSLogs/<ACCOUNT_ID>/CloudTrail/
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/8-check-awslogs-prefix.png" title="Kiểm tra AWSLogs prefix" >}}

Ghi lại:

```text
AUDIT_BUCKET_NAME=<TÊN_BUCKET_DO_CLOUDTRAIL_TẠO>
```

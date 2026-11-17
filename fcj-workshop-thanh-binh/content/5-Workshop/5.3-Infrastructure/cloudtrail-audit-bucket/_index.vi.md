---
title: "CloudTrail và S3 audit bucket"
date: 2026-11-15
weight: 13
chapter: false
---

## 1. Tạo trail

Mở CloudTrail → **Trails → Create trail**.

![Mở CloudTrail và tạo trail](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/1-open-cloudtrail-and-create-trail.png)

Nhập:

```text
Trail name: nexusoms-demo-trail
Storage location: Create new S3 bucket
Log file SSE-KMS encryption: Disabled
Log file validation: Enabled
CloudWatch Logs: Disabled
```

![Cấu hình trail](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/2-configure-trail.png)

Trong **Choose log events**, chọn:

```text
Event type: Management events
API activity: Read và Write
```

Giữ các mục còn lại ở trạng thái tắt.

![Chọn management events](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/3-configure-management-events.png)

Chọn **Create trail**.

![Tạo trail](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/4-create-trail.png)

## 2. Kiểm tra audit bucket

Mở trail, ghi lại tên S3 bucket được CloudTrail tạo rồi mở bucket.

![Mở audit bucket](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/5-open-audit-bucket.png)

Trong **Permissions**, xác nhận **Block all public access** đang bật.

![Kiểm tra Block Public Access](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/6-check-block-public-access.png)

Trong **Properties**, xác nhận default encryption là **SSE-S3**.

![Kiểm tra encryption](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/7-check-default-encryption.png)

Sau vài phút, bucket phải có prefix:

```text
AWSLogs/<ACCOUNT_ID>/CloudTrail/
```

![Kiểm tra AWSLogs prefix](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/8-check-awslogs-prefix.png)

Ghi lại:

```text
AUDIT_BUCKET_NAME=<TÊN_BUCKET_DO_CLOUDTRAIL_TẠO>
```

Không đổi tên bucket. Tên này được dùng lại khi cấu hình ALB access logs.

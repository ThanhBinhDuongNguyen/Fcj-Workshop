---
title: "ALB access logs"
date: 2026-11-15
weight: 5
chapter: false
---

## 1. Cập nhật audit bucket policy

Mở S3 → `<AUDIT_BUCKET_NAME>`.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/1-open-audit-bucket.png" title="Mở audit bucket" >}}

Chọn **Permissions → Bucket policy → Edit**.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/2-open-bucket-policy.png" title="Mở bucket policy" >}}

Giữ nguyên các statement CloudTrail. Thêm object sau vào mảng `Statement`:

```json
{
  "Sid": "AllowAlbAccessLogDelivery",
  "Effect": "Allow",
  "Principal": {
    "Service": "logdelivery.elasticloadbalancing.amazonaws.com"
  },
  "Action": "s3:PutObject",
  "Resource": "arn:aws:s3:::<AUDIT_BUCKET_NAME>/alb-access/AWSLogs/<ACCOUNT_ID>/*",
  "Condition": {
    "StringEquals": {
      "aws:SourceAccount": "<ACCOUNT_ID>"
    },
    "ArnLike": {
      "aws:SourceArn": "arn:aws:elasticloadbalancing:ap-southeast-1:<ACCOUNT_ID>:loadbalancer/*"
    }
  }
}
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/3-add-alb-log-delivery-policy.png" title="Thêm ALB log delivery policy" >}}

## 2. Bật access logs

Mở EC2 → **Load balancers → nexusoms-demo-alb → Attributes → Edit**.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/4-open-alb-attributes.png" title="Mở ALB attributes" >}}

```text
Logging → Access logs: Enabled
S3 URI: s3://<AUDIT_BUCKET_NAME>/alb-access
```

Chọn **Save changes**.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/5-enable-alb-access-logs.png" title="Bật ALB access logs" >}}

Kiểm tra bucket xuất hiện:

```text
alb-access/AWSLogs/<ACCOUNT_ID>/ELBAccessLogTestFile
```

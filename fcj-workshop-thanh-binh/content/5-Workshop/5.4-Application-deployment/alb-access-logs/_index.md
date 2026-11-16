---
title: "ALB access logs"
date: 2026-11-15
weight: 5
chapter: false
---

## 1. Update the audit bucket policy

Open S3 → `<AUDIT_BUCKET_NAME>`.

![Open the audit bucket](/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/1-open-audit-bucket.png)

Select **Permissions → Bucket policy → Edit**.

![Open the bucket policy](/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/2-open-bucket-policy.png)

Keep the CloudTrail statements. Add this object to the `Statement` array:

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

![Add the ALB log delivery policy](/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/3-add-alb-log-delivery-policy.png)

## 2. Enable access logs

Open EC2 → **Load balancers → nexusoms-demo-alb → Attributes → Edit**.

![Open ALB attributes](/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/4-open-alb-attributes.png)

```text
Logging → Access logs: Enabled
S3 URI: s3://<AUDIT_BUCKET_NAME>/alb-access
```

Select **Save changes**.

![Enable ALB access logs](/images/5-Workshop/nexusoms/5.4-Application-deployment/alb-access-logs/5-enable-alb-access-logs.png)

Verify this test object in the bucket:

```text
alb-access/AWSLogs/<ACCOUNT_ID>/ELBAccessLogTestFile
```

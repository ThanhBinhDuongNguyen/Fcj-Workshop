---
title: "CloudTrail and S3 audit bucket"
date: 2026-11-15
weight: 12
chapter: false
---

## 1. Create the trail

Open CloudTrail → **Trails → Create trail**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/1-open-cloudtrail-and-create-trail.png" title="Open CloudTrail and create a trail" >}}

```text
Trail name: nexusoms-demo-trail
Storage location: Create new S3 bucket
Log file SSE-KMS encryption: Disabled
Log file validation: Enabled
CloudWatch Logs: Disabled
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/2-configure-trail.png" title="Configure the trail" >}}

Under **Choose log events**, select:

```text
Event type: Management events
API activity: Read and Write
```

Leave the remaining event types disabled.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/3-configure-management-events.png" title="Configure management events" >}}

Select **Create trail**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/4-create-trail.png" title="Create the trail" >}}

## 2. Verify the audit bucket

Open the trail, record the S3 bucket name created by CloudTrail, and open that bucket.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/5-open-audit-bucket.png" title="Open the audit bucket" >}}

Under **Permissions**, verify that **Block all public access** is enabled.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/6-check-block-public-access.png" title="Verify Block Public Access" >}}

Under **Properties**, verify that default encryption is **SSE-S3**.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/7-check-default-encryption.png" title="Verify default encryption" >}}

After a few minutes, verify this prefix:

```text
AWSLogs/<ACCOUNT_ID>/CloudTrail/
```

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/8-check-awslogs-prefix.png" title="Verify the AWSLogs prefix" >}}

```text
AUDIT_BUCKET_NAME=<BUCKET_CREATED_BY_CLOUDTRAIL>
```

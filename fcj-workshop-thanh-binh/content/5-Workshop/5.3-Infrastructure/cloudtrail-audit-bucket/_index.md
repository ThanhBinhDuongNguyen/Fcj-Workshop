---
title: "CloudTrail and S3 audit bucket"
date: 2026-11-15
weight: 13
chapter: false
---

## 1. Create the trail

Open CloudTrail → **Trails → Create trail**.

![Open CloudTrail and create a trail](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/1-open-cloudtrail-and-create-trail.png)

```text
Trail name: nexusoms-demo-trail
Storage location: Create new S3 bucket
Log file SSE-KMS encryption: Disabled
Log file validation: Enabled
CloudWatch Logs: Disabled
```

![Configure the trail](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/2-configure-trail.png)

Under **Choose log events**, select:

```text
Event type: Management events
API activity: Read and Write
```

Leave the remaining event types disabled.

![Configure management events](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/3-configure-management-events.png)

Select **Create trail**.

![Create the trail](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/4-create-trail.png)

## 2. Verify the audit bucket

Open the trail, record the S3 bucket name created by CloudTrail, and open that bucket.

![Open the audit bucket](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/5-open-audit-bucket.png)

Under **Permissions**, verify that **Block all public access** is enabled.

![Verify Block Public Access](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/6-check-block-public-access.png)

Under **Properties**, verify that default encryption is **SSE-S3**.

![Verify default encryption](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/7-check-default-encryption.png)

After a few minutes, verify this prefix:

```text
AWSLogs/<ACCOUNT_ID>/CloudTrail/
```

![Verify the AWSLogs prefix](/images/5-Workshop/nexusoms/5.3-Infrastructure/cloudtrail-audit-bucket/8-check-awslogs-prefix.png)

```text
AUDIT_BUCKET_NAME=<BUCKET_CREATED_BY_CLOUDTRAIL>
```

Do not rename the bucket. Use this name again when configuring ALB access logs.

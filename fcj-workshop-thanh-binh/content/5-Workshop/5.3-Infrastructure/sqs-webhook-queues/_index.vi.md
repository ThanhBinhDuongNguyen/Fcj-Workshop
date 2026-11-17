---
title: "SQS FIFO và Dead-letter Queue"
date: 2026-11-17
weight: 12
chapter: false
---

Tạo dead-letter queue trước, sau đó tạo webhook queue và gắn redrive policy.

## 1. Tạo Dead-letter Queue

Mở **Amazon SQS → Create queue**.

![Mở Amazon SQS](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/1-open-sqs-dashboard.png)

Nhập:

```text
Type: FIFO
Name: nexusoms-demo-webhook-dlq.fifo
```

![Chọn loại và tên DLQ](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/2-configure-dlq-type-and-name.png)

Đặt cấu hình:

```text
Visibility timeout: 60 seconds
Message retention period: 4 days
Content-based deduplication: Disabled
```

![Cấu hình DLQ](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/3-configure-dlq-settings.png)

Chọn mã hóa bằng **Amazon SQS managed key**. Giữ access policy ở chế độ **Basic**.

![Cấu hình mã hóa DLQ](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/4-configure-dlq-encryption.png)

Không bật Dead-letter queue cho queue này.

![Kiểm tra cấu hình DLQ](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/5-review-dlq-settings.png)

Chọn **Create queue**.

![Tạo DLQ thành công](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/6-create-webhook-dlq.png)

## 2. Tạo Webhook Queue

Chọn **Create queue** và nhập:

```text
Type: FIFO
Name: nexusoms-demo-webhook-queue.fifo
Visibility timeout: 60 seconds
Message retention period: 1 day
Delivery delay: 0 seconds
Receive message wait time: 20 seconds
Content-based deduplication: Disabled
High throughput FIFO: Disabled
Encryption: Amazon SQS managed key
Access policy: Basic
```

![Cấu hình webhook queue](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/7-configure-webhook-queue.png)

![Cấu hình thời gian webhook queue](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/8-configure-webhook-queue-settings.png)

Bật **Dead-letter queue**:

```text
Dead-letter queue: nexusoms-demo-webhook-dlq.fifo
Maximum receives: 3
```

![Liên kết dead-letter queue](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/9-attach-dead-letter-queue.png)

Chọn **Create queue**.

## 3. Lấy Queue URL và ARN

Chạy toàn bộ khối PowerShell:

```powershell
$AwsRegion = "ap-southeast-1"
$AwsProfile = "nexusoms-demo"
$WebhookQueueName = "nexusoms-demo-webhook-queue.fifo"
$WebhookDlqName = "nexusoms-demo-webhook-dlq.fifo"

$WebhookQueueUrl = aws sqs get-queue-url `
  --profile $AwsProfile `
  --region $AwsRegion `
  --queue-name $WebhookQueueName `
  --query QueueUrl `
  --output text

$WebhookDlqUrl = aws sqs get-queue-url `
  --profile $AwsProfile `
  --region $AwsRegion `
  --queue-name $WebhookDlqName `
  --query QueueUrl `
  --output text

$WebhookQueueArn = aws sqs get-queue-attributes `
  --profile $AwsProfile `
  --region $AwsRegion `
  --queue-url $WebhookQueueUrl `
  --attribute-names QueueArn `
  --query "Attributes.QueueArn" `
  --output text

$WebhookDlqArn = aws sqs get-queue-attributes `
  --profile $AwsProfile `
  --region $AwsRegion `
  --queue-url $WebhookDlqUrl `
  --attribute-names QueueArn `
  --query "Attributes.QueueArn" `
  --output text

"WEBHOOK_QUEUE_URL=$WebhookQueueUrl"
"WEBHOOK_QUEUE_ARN=$WebhookQueueArn"
"WEBHOOK_DLQ_URL=$WebhookDlqUrl"
"WEBHOOK_DLQ_ARN=$WebhookDlqArn"
```

Lưu hai Queue URL để nhập vào ECS Task Definition.

## 4. Cấp quyền cho ECS Task Role

Mở **IAM → Roles → nexusoms-demo-ecs-task-role**.

![Mở ECS task role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/8-open-ecs-task-role.png)

Chọn **Add permissions → Create inline policy → JSON**.

![Tạo SQS inline policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/9-add-sqs-inline-policy.png)

Thay `<ACCOUNT_ID>` rồi dán policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "UseWebhookQueue",
      "Effect": "Allow",
      "Action": [
        "sqs:SendMessage",
        "sqs:ReceiveMessage",
        "sqs:DeleteMessage",
        "sqs:GetQueueAttributes"
      ],
      "Resource": "arn:aws:sqs:ap-southeast-1:<ACCOUNT_ID>:nexusoms-demo-webhook-queue.fifo"
    },
    {
      "Sid": "ReadWebhookDeadLetterQueue",
      "Effect": "Allow",
      "Action": "sqs:GetQueueAttributes",
      "Resource": "arn:aws:sqs:ap-southeast-1:<ACCOUNT_ID>:nexusoms-demo-webhook-dlq.fifo"
    }
  ]
}
```

Lưu policy với tên `UseNexusOmsWebhookQueues`. Gắn policy vào task role, không gắn vào execution role.

## 5. Kiểm tra

```text
Main queue: nexusoms-demo-webhook-queue.fifo
DLQ: nexusoms-demo-webhook-dlq.fifo
Maximum receives: 3
Task role policy: UseNexusOmsWebhookQueues
```

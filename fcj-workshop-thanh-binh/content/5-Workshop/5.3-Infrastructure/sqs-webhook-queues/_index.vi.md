---
title: "SQS FIFO và Dead-letter Queue"
date: 2026-11-16
weight: 12
chapter: false
---

Tạo dead-letter queue trước, sau đó tạo webhook queue và gắn redrive policy.

## 1. Tạo Dead-letter Queue

Mở **Amazon SQS → Create queue** và nhập:

```text
Type: FIFO
Name: nexusoms-demo-webhook-dlq.fifo
Visibility timeout: 60 seconds
Message retention period: 4 days
Content-based deduplication: Disabled
Encryption: Server-side encryption with Amazon SQS managed key (SSE-SQS)
Access policy: Basic
Dead-letter queue: Disabled
```

Chọn **Create queue**.

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
Encryption: Server-side encryption with Amazon SQS managed key (SSE-SQS)
Access policy: Basic
Dead-letter queue: Enabled
Dead-letter queue: nexusoms-demo-webhook-dlq.fifo
Maximum receives: 3
```

Chọn **Create queue**.

Hai queue phải cùng tài khoản AWS, cùng Region `ap-southeast-1` và có tên kết thúc bằng `.fifo`.

## 3. Lấy Queue URL và ARN

Chạy PowerShell:

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

Mở **IAM → Roles → nexusoms-demo-ecs-task-role → Add permissions → Create inline policy → JSON**.

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

Lưu policy với tên:

```text
UseNexusOmsWebhookQueues
```

Không gắn policy này vào execution role.

## 5. Kiểm tra

Xác nhận:

```text
Main queue: nexusoms-demo-webhook-queue.fifo
DLQ: nexusoms-demo-webhook-dlq.fifo
Maximum receives: 3
Task role policy: UseNexusOmsWebhookQueues
```


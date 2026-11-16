---
title: "SQS FIFO and dead-letter queues"
date: 2026-11-16
weight: 12
chapter: false
---

Create the dead-letter queue first, then create the webhook queue and attach the redrive policy.

## 1. Create the dead-letter queue

Open **Amazon SQS → Create queue** and enter:

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

Choose **Create queue**.

## 2. Create the webhook queue

Choose **Create queue** and enter:

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

Choose **Create queue**.

Both queues must use the same AWS account and `ap-southeast-1` Region. FIFO queue names must end with `.fifo`.

## 3. Get queue URLs and ARNs

Run PowerShell:

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

Keep both queue URLs for the ECS task definition.

## 4. Grant the ECS task role access

Open **IAM → Roles → nexusoms-demo-ecs-task-role → Add permissions → Create inline policy → JSON**.

Replace `<ACCOUNT_ID>` and paste:

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

Save the policy as:

```text
UseNexusOmsWebhookQueues
```

Do not attach this policy to the execution role.

## 5. Verify

Confirm:

```text
Main queue: nexusoms-demo-webhook-queue.fifo
DLQ: nexusoms-demo-webhook-dlq.fifo
Maximum receives: 3
Task role policy: UseNexusOmsWebhookQueues
```


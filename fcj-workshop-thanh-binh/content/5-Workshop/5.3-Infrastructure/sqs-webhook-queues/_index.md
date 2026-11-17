---
title: "SQS FIFO and Dead-letter Queue"
date: 2026-11-17
weight: 12
chapter: false
---

Create the dead-letter queue first. Then create the webhook queue and attach the redrive policy.

## 1. Create the Dead-letter Queue

Open **Amazon SQS → Create queue**.

![Open Amazon SQS](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/1-open-sqs-dashboard.png)

Use these values:

```text
Type: FIFO
Name: nexusoms-demo-webhook-dlq.fifo
Visibility timeout: 60 seconds
Message retention period: 4 days
Content-based deduplication: Disabled
Encryption: Amazon SQS managed key
Access policy: Basic
Dead-letter queue: Disabled
```

![Set the DLQ name](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/2-configure-dlq-type-and-name.png)

![Configure the DLQ](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/3-configure-dlq-settings.png)

![Configure DLQ encryption](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/4-configure-dlq-encryption.png)

![Review the DLQ](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/5-review-dlq-settings.png)

Choose **Create queue**.

![DLQ created](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/6-create-webhook-dlq.png)

## 2. Create the Webhook Queue

Choose **Create queue** and use these values:

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
Dead-letter queue: nexusoms-demo-webhook-dlq.fifo
Maximum receives: 3
```

![Configure the webhook queue](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/7-configure-webhook-queue.png)

![Configure webhook queue timing](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/8-configure-webhook-queue-settings.png)

![Attach the dead-letter queue](/images/5-Workshop/nexusoms/5.3-Infrastructure/sqs-webhook-queues/9-attach-dead-letter-queue.png)

Choose **Create queue**.

## 3. Get the Queue URLs and ARNs

Run the complete PowerShell block:

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

Save both Queue URLs for the ECS task definition.

## 4. Grant Access to the ECS Task Role

Open **IAM → Roles → nexusoms-demo-ecs-task-role**.

![Open the ECS task role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/8-open-ecs-task-role.png)

Choose **Add permissions → Create inline policy → JSON**.

![Create the SQS inline policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/9-add-sqs-inline-policy.png)

Replace `<ACCOUNT_ID>` and paste this policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "UseWebhookQueue",
      "Effect": "Allow",
      "Action": ["sqs:SendMessage", "sqs:ReceiveMessage", "sqs:DeleteMessage", "sqs:GetQueueAttributes"],
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

Save it as `UseNexusOmsWebhookQueues`. Attach it to the task role, not the execution role.

## 5. Verify

```text
Main queue: nexusoms-demo-webhook-queue.fifo
DLQ: nexusoms-demo-webhook-dlq.fifo
Maximum receives: 3
Task role policy: UseNexusOmsWebhookQueues
```

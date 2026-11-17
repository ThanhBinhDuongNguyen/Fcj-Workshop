---
title: "ECS Task Definition"
date: 2026-11-13
weight: 7
chapter: false
---

Tạo task definition cho API, đọc cấu hình bí mật từ Secrets Manager và gửi log đến CloudWatch.

## Bước 1: Tạo CloudWatch log group

Chạy trong PowerShell:

```powershell
$AwsProfile = "nexusoms-demo"
$AwsRegion = "ap-southeast-1"
$LogGroupName = "/ecs/nexusoms-demo-api"

aws logs create-log-group `
  --profile $AwsProfile `
  --region $AwsRegion `
  --log-group-name $LogGroupName

aws logs put-retention-policy `
  --profile $AwsProfile `
  --region $AwsRegion `
  --log-group-name $LogGroupName `
  --retention-in-days 7
```

Nếu log group đã tồn tại, tiếp tục bước sau.

## Bước 2: Mở trang tạo task definition

Mở **Amazon ECS → Task definitions → Create new task definition**.

![Mở trang tạo task definition](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/1-open-task-definitions-dashboard-create-new-task-definition.png)

## Bước 3: Cấu hình task definition

Nhập các giá trị sau:

```text
Task definition family: nexusoms-demo-api
Launch type: AWS Fargate
Operating system: Linux
CPU architecture: X86_64
CPU: 0.5 vCPU
Memory: 1 GB
Task role: nexusoms-demo-ecs-task-role
Task execution role: nexusoms-demo-ecs-execution-role
```

![Cấu hình task definition](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/2-configure-task-definition.png)

![Chọn hệ điều hành và IAM role](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/3-configure-os-task-roles.png)

## Bước 4: Cấu hình container

Nhập full ECR image URI có kèm tag mới.

```text
Container name: api
Container port: 8080
Protocol: TCP
```

![Cấu hình container](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/4-configure-container-1-detail.png)

## Bước 5: Thêm biến môi trường

Trong **Environment variables**, thêm các giá trị có **Value type = Value**:

```text
ASPNETCORE_ENVIRONMENT=Production
ASPNETCORE_HTTP_PORTS=8080
AWS__Region=ap-southeast-1
WebhookQueue__Mode=Sqs
WebhookQueue__QueueUrl=https://sqs.ap-southeast-1.amazonaws.com/<ACCOUNT_ID>/nexusoms-demo-webhook-queue.fifo
WebhookQueue__DeadLetterQueueUrl=https://sqs.ap-southeast-1.amazonaws.com/<ACCOUNT_ID>/nexusoms-demo-webhook-dlq.fifo
WebhookQueue__WaitTimeSeconds=20
```

Hai giá trị queue phải là **Queue URL**, không phải ARN.

Thêm bốn dòng có **Value type = ValueFrom** và chọn đúng JSON key trong secret `nexusoms/demo/application`:

| Key trong task definition | JSON key trong secret |
| --- | --- |
| `ConnectionStrings__Postgres` | `postgresConnectionString` |
| `Supabase__Url` | `supabaseUrl` |
| `WebhookSimulator__Secret` | `webhookSigningSecret` |
| `WebhookSimulator__SystemActorId` | `systemActorId` |

![Cấu hình biến môi trường](/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/5-configure-environment-variables.png)

## Bước 6: Cấu hình log

Bật **Use log collection** và nhập:

```text
Destination: Amazon CloudWatch
awslogs-group: /ecs/nexusoms-demo-api
awslogs-region: ap-southeast-1
awslogs-stream-prefix: ecs
awslogs-create-group: false
```

Chọn **Create** và ghi lại revision mới, ví dụ `nexusoms-demo-api:1`.

---
title: "IAM role cho ECS"
date: 2026-11-20
weight: 11
chapter: false
---

Tạo execution role cho ECS và task role cho ứng dụng.

## Bước 1: Tạo execution role

Mở **IAM → Roles → Create role**.

![Mở trang tạo IAM role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/1-open-roles-dashboard-create-role.png)

Chọn:

```text
Trusted entity type: AWS service
Service or use case: Elastic Container Service Task
```

![Chọn trusted entity](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/2-configure-select-trusted-entity.png)

Gắn policy:

```text
AmazonECSTaskExecutionRolePolicy
```

![Gắn execution policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/3-configure-permissions.png)

Nhập role name rồi tạo role:

```text
nexusoms-demo-ecs-execution-role
```

![Nhập tên execution role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/4-fill-name-role.png)

## Bước 2: Cho execution role đọc secret

Mở role vừa tạo → **Add permissions → Create inline policy → JSON**.

![Mở execution role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/5-find-role-open-role.png)

![Tạo inline policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/6-create-inline-policy.png)

Thay `<SECRET_ARN>` bằng ARN đầy đủ của secret:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "ReadNexusOmsApplicationSecret",
      "Effect": "Allow",
      "Action": "secretsmanager:GetSecretValue",
      "Resource": "<SECRET_ARN>"
    }
  ]
}
```

![Dán JSON policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/7-paste-the-json-to-create.png)

Lưu policy với tên:

```text
ReadNexusOmsApplicationSecret
```

`Resource` sử dụng ARN của secret, không thêm `:jsonKey::`. Hậu tố JSON key chỉ được dùng tại `ValueFrom` của Task Definition.

## Bước 3: Tạo task role

Tạo role thứ hai với cùng trusted entity:

```text
nexusoms-demo-ecs-task-role
```

Không gắn `AdministratorAccess`. Quyền SQS sẽ được thêm sau khi tạo queue.

## Bước 4: Kiểm tra role

```powershell
$AwsProfile = "nexusoms-demo"
$AwsRegion = "ap-southeast-1"
$SecretArn = aws secretsmanager describe-secret --profile $AwsProfile --region $AwsRegion --secret-id nexusoms/demo/application --query ARN --output text

aws iam list-attached-role-policies --profile $AwsProfile --role-name nexusoms-demo-ecs-execution-role
aws iam list-role-policies --profile $AwsProfile --role-name nexusoms-demo-ecs-execution-role
aws iam get-role-policy --profile $AwsProfile --role-name nexusoms-demo-ecs-execution-role --policy-name ReadNexusOmsApplicationSecret --query "PolicyDocument.Statement[?Action=='secretsmanager:GetSecretValue'].Resource" --output text
$SecretArn
aws iam get-role --profile $AwsProfile --role-name nexusoms-demo-ecs-task-role --query "Role.AssumeRolePolicyDocument.Statement[].Principal.Service" --output text
```

Kết quả cần đạt:

- Execution role có `AmazonECSTaskExecutionRolePolicy` và `ReadNexusOmsApplicationSecret`.
- ARN từ `get-role-policy` phải giống `$SecretArn`. ARN trong IAM không thêm `:jsonKey::`.
- Task role tin cậy `ecs-tasks.amazonaws.com`.

Nếu vừa sửa policy, chạy **Force new deployment** và kiểm tra task mới. Stopped reason của task cũ không phản ánh policy vừa cập nhật.

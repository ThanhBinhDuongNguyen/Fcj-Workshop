---
title: "ECS IAM roles"
date: 2026-11-20
weight: 11
chapter: false
---

Create an execution role for ECS and a task role for the application.

## Step 1: Create the execution role

Open **IAM → Roles → Create role**.

![Open the IAM role creation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/1-open-roles-dashboard-create-role.png)

```text
Trusted entity type: AWS service
Service or use case: Elastic Container Service Task
```

![Select the trusted entity](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/2-configure-select-trusted-entity.png)

Attach:

```text
AmazonECSTaskExecutionRolePolicy
```

![Attach the execution policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/3-configure-permissions.png)

Enter the role name and create the role:

```text
nexusoms-demo-ecs-execution-role
```

![Enter the execution role name](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/4-fill-name-role.png)

## Step 2: Allow the execution role to read the secret

Open the new role → **Add permissions → Create inline policy → JSON**.

![Open the execution role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/5-find-role-open-role.png)

![Create an inline policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/6-create-inline-policy.png)

Replace `<SECRET_ARN>` with the complete secret ARN:

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

![Paste the JSON policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/7-paste-the-json-to-create.png)

Save the policy as:

```text
ReadNexusOmsApplicationSecret
```

Use the secret ARN in `Resource`. Add the `:jsonKey::` suffix only in the Task Definition `ValueFrom` field.

## Step 3: Create the task role

Create a second role with the same trusted entity:

```text
nexusoms-demo-ecs-task-role
```

Do not attach `AdministratorAccess`. Add the SQS permissions after creating the queues.

## Step 4: Verify the roles

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

Expected result:

- The execution role has `AmazonECSTaskExecutionRolePolicy` and `ReadNexusOmsApplicationSecret`.
- The ARN returned by `get-role-policy` matches `$SecretArn`. The IAM resource ARN does not include `:jsonKey::`.
- The task role trusts `ecs-tasks.amazonaws.com`.

After changing the policy, run **Force new deployment** and inspect the new task. The stopped reason of an older task does not reflect the updated policy.

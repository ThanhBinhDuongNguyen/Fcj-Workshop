---
title: "IAM role cho ECS"
date: 2026-11-13
weight: 11
chapter: false
---

Execution role cho phép ECS kéo image, ghi log và đọc secret. Task role được tạo trước; quyền SQS sẽ được gắn sau khi tạo hai queue.

## 7.3. Tạo IAM execution role và task role

Mở **IAM → Roles → Create role**.

**Execution role**

1\. Trusted entity: **AWS service** → **Elastic Container Service** → **Elastic Container Service Task**.

2\. Gắn policy AWS-managed AmazonECSTaskExecutionRolePolicy.

3\. Tên role: nexusoms-demo-ecs-execution-role; tạo role.

4\. Mở role → **Add permissions → Create inline policy → JSON** và dán policy sau. Thay &lt;SECRET_ARN&gt; bằng ARN đầy đủ đã copy, gồm hậu tố ngẫu nhiên sau tên secret.

```
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

5\. Next → Policy name ReadNexusOmsApplicationSecret → Create policy.

**Task role**

1\. Tạo role mới với cùng trusted entity **Elastic Container Service Task**.

2\. Không gắn AdministratorAccess và chưa chọn permissions policy tại bước này.

3\. Role name nexusoms-demo-ecs-task-role; tạo role. Quyền SQS được thêm ở trang tiếp theo sau khi đã có queue ARN.

Execution role là quyền ECS dùng để tải image, ghi log và lấy secret khi task khởi động. Task role là quyền ứng dụng .NET dùng để gửi, nhận, xóa và đọc trạng thái SQS. Không tráo hai role.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Elastic Container Service Task**

```text
nexusoms-demo-ecs-execution-role
```

**Secret name**

```text
ReadNexusOmsApplicationSecret
```

**Elastic Container Service Task**

```text
nexusoms-demo-ecs-task-role
```

## Thao tác trên AWS Console

### Bước 1 - Open Roles dashboard + Create role


![Open Roles dashboard + Create role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/1-open-roles-dashboard-create-role.png)

### Bước 2 - Configure Select trusted entity

![Configure Select trusted entity](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/2-configure-select-trusted-entity.png)

### Bước 3 - Configure permissions

![Configure permissions](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/3-configure-permissions.png)

### Bước 4 - Fill name role

**Giá trị dùng ở bước này**

**Elastic Container Service Task**

```text
nexusoms-demo-ecs-execution-role
```

**Secret name**

```text
ReadNexusOmsApplicationSecret
```

**Elastic Container Service Task**

```text
nexusoms-demo-ecs-task-role
```

![Fill name role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/4-fill-name-role.png)

### Bước 5 - Find role + Open role

![Find role + Open role](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/5-find-role-open-role.png)

### Bước 6 - Create inline policy

![Create inline policy](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/6-create-inline-policy.png)

### Bước 7 - Paste the json to create

![Paste the json to create](/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/7-paste-the-json-to-create.png)

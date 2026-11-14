---
title: "IAM role cho ECS"
date: 2026-11-13
weight: 10
chapter: false
---

Execution role cho phép ECS kéo image, ghi log và đọc secret. Task role giữ tối thiểu vì ứng dụng hiện không gọi AWS API trực tiếp.

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

2\. Không gắn AdministratorAccess; code hiện tại không cần quyền gọi AWS API trực tiếp.

3\. Role name nexusoms-demo-ecs-task-role; tạo role.

Execution role là quyền ECS dùng để tải image, ghi log và lấy secret khi task khởi động. Task role là quyền cấp cho ứng dụng .NET đang chạy trong container. Không tráo hai role.

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

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Roles dashboard + Create role

Trên AWS Console, thực hiện **Open Roles dashboard + Create role** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/1-open-roles-dashboard-create-role.png" title="Open Roles dashboard + Create role" >}}

### Bước 2 - Configure Select trusted entity

Trên AWS Console, thực hiện **Configure Select trusted entity** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/2-configure-select-trusted-entity.png" title="Configure Select trusted entity" >}}

### Bước 3 - Configure permissions

Trên AWS Console, thực hiện **Configure permissions** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/3-configure-permissions.png" title="Configure permissions" >}}

### Bước 4 - Fill name role

Trên AWS Console, thực hiện **Fill name role** như hình dưới đây.

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

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/4-fill-name-role.png" title="Fill name role" >}}

### Bước 5 - Find role + Open role

Trên AWS Console, thực hiện **Find role + Open role** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/5-find-role-open-role.png" title="Find role + Open role" >}}

### Bước 6 - Create inline policy

Trên AWS Console, thực hiện **Create inline policy** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/6-create-inline-policy.png" title="Create inline policy" >}}

### Bước 7 - Paste the json to create

Trên AWS Console, thực hiện **Paste the json to create** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.3-Infrastructure/ecs-iam-roles/7-paste-the-json-to-create.png" title="Paste the json to create" >}}

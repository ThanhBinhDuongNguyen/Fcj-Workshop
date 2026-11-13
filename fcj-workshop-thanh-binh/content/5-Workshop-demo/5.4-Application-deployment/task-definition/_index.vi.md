---
title: "ECS Task Definition"
date: 2026-11-13
weight: 6
chapter: false
---

Bốn biến bí mật phải dùng **ValueFrom** và trỏ đúng JSON key riêng trong cùng secret. Không dán cả JSON object làm giá trị.

## 9.3. Tạo task definition

Mở **Amazon ECS → Task definitions → Create new task definition**. Trên giao diện mới, phần biến môi trường nằm trong container definition; không tìm "Add environment variable from Secrets Manager" ở tab khác.

1\. Family: nexusoms-demo-api.

2\. Launch type/infrastructure: AWS Fargate; OS Linux; architecture X86_64.

3\. CPU 0.5 vCPU (512); Memory 1 GB (1024 MiB).

4\. Task role: nexusoms-demo-ecs-task-role.

5\. Task execution role: nexusoms-demo-ecs-execution-role.

6\. Container name api; Image URI dán full ECR URI gồm tag mới.

7\. Port mapping: container port 8080, protocol TCP.

8\. Environment variables → Add individually: Key ASPNETCORE_ENVIRONMENT, Value Production.

9\. Environment variables → Add individually → chọn **Value type: ValueFrom**. Với mỗi dòng, Key là tên biến môi trường; ValueFrom chọn Secret nexusoms/demo/application và đúng JSON key:

| **Key trong task definition**     | **JSON key cần chọn trong secret** |
| --------------------------------- | ---------------------------------- |
| `ConnectionStrings__Postgres`     | `postgresConnectionString`         |
| `Supabase__Url`                   | `supabaseUrl`                      |
| `WebhookSimulator__Secret`        | `webhookSigningSecret`             |
| `WebhookSimulator__SystemActorId` | `systemActorId`                    |

Mỗi dòng đều có **Value type = ValueFrom**, nhưng phải chọn đúng JSON key riêng của dòng đó. Không nhập cùng nexusoms/demo/application làm giá trị plain text; không trỏ cả bốn biến vào cùng một JSON object. Tên bên trái phải khớp chính xác, gồm hai dấu gạch dưới \__.

10\. Logging: driver awslogs, log group /ecs/nexusoms-demo-api, region ap-southeast-1, stream prefix ecs.

11\. Tạo task definition và ghi revision mới, ví dụ nexusoms-demo-api:1.

Nếu AWS báo execution role không thể đọc secret, mở đúng execution role và xác nhận policy ReadNexusOmsApplicationSecret có ARN trùng Secret ARN. Nếu vừa sửa secret, ECS không tự thay env trong task đang chạy; cần deploy task mới.

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**ECS Task Definition - giá trị 1**

```text
nexusoms-demo-api
```

**CPU**

```text
CPU: 0.5 vCPU
```

**Memory**

```text
Memory: 1 GB
```

**Container port**

```text
Container port: 8080
```

**ECS Task Definition - giá trị 5**

```text
ASPNETCORE_ENVIRONMENT=Production
```

**Connection string**

```text
ConnectionStrings__Postgres <- postgresConnectionString
```

**Supabase value**

```text
Supabase__Url <- supabaseUrl
```

**System actor UUID**

```text
WebhookSimulator__SystemActorId <- systemActorId
```

**Webhook signing secret**

```text
WebhookSimulator__SigningSecret <- webhookSigningSecret
```

**ECS Task Definition - giá trị 10**

```text
/ecs/nexusoms-demo-api
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open task definitions dashboard + Create new task definition

Trên AWS Console, thực hiện **Open task definitions dashboard + Create new task definition** như hình dưới đây.


{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/1. Open task definitions dashboard + Create new task definition.png" title="Open task definitions dashboard + Create new task definition" >}}

### Bước 2 - Configure task definition

Trên AWS Console, thực hiện **Configure task definition** như hình dưới đây.

**Giá trị dùng ở bước này**

**ECS Task Definition - giá trị 1**

```text
nexusoms-demo-api
```

**CPU**

```text
CPU: 0.5 vCPU
```

**Memory**

```text
Memory: 1 GB
```

**ECS Task Definition - giá trị 5**

```text
ASPNETCORE_ENVIRONMENT=Production
```

**Connection string**

```text
ConnectionStrings__Postgres <- postgresConnectionString
```

**Supabase value**

```text
Supabase__Url <- supabaseUrl
```

**System actor UUID**

```text
WebhookSimulator__SystemActorId <- systemActorId
```

**Webhook signing secret**

```text
WebhookSimulator__SigningSecret <- webhookSigningSecret
```

**ECS Task Definition - giá trị 10**

```text
/ecs/nexusoms-demo-api
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/2. Configure task definition.png" title="Configure task definition" >}}

### Bước 3 - Configure OS, task roles

Trên AWS Console, thực hiện **Configure OS, task roles** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/3. Configure OS, task roles.png" title="Configure OS, task roles" >}}

### Bước 4 - Configure Container-1 detail

Trên AWS Console, thực hiện **Configure Container-1 detail** như hình dưới đây.

**Giá trị dùng ở bước này**

**Container port**

```text
Container port: 8080
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/4. Configure Container-1 detail.png" title="Configure Container-1 detail" >}}

### Bước 5 - Configure environment variables

Trên AWS Console, thực hiện **Configure environment variables** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/task-definition/5. Configure environment variables.png" title="Configure environment variables" >}}

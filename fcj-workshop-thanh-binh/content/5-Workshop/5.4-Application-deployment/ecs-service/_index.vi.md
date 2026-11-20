---
title: "ECS Service"
date: 2026-11-13
weight: 8
chapter: false
---

Tạo ECS Service chạy task Fargate trong hai private subnet và đăng ký task với ALB target group.

## Bước 1: Tạo service

Mở **Amazon ECS → Clusters → nexusoms-demo-cluster → Services → Create**.

![Mở trang tạo ECS Service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/1-choose-service-tab-create.png)

## Bước 2: Cấu hình service

```text
Compute options: Launch type
Launch type: Fargate
Task definition family: nexusoms-demo-api
Service name: nexusoms-demo-api-service
Desired tasks: 1
```

Chọn revision task definition mới nhất. Khi cần trình diễn hai task, tăng **Desired tasks** lên `2`.

Với `Desired tasks = 1`, task chỉ chạy trong một Availability Zone tại một thời điểm. Việc chọn hai private subnet chưa tạo backend HA hai AZ.

![Cấu hình ECS Service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/2-configure-service-detail.png)

## Bước 3: Cấu hình mạng

```text
VPC: nexusoms-demo-vpc
Subnets: private-a và private-b
Security Group: nexusoms-demo-task-sg
Auto-assign public IP: Disabled
```

![Cấu hình mạng cho service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/3-configure-network-for-service.png)

## Bước 4: Kết nối Load Balancer

```text
Load balancer type: Application Load Balancer
Load balancer: nexusoms-demo-alb
Listener: HTTP:80
Target group: nexusoms-demo-api-tg
Container: api
Container port: 8080
```

![Chọn Application Load Balancer](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4-configure-load-balancer-for-service.png)

![Chọn listener và target group](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/4-1-configure-load-balancer-for-service.png)

## Bước 5: Hoàn tất

Chọn **Create** và chờ deployment ổn định.

![Tạo ECS Service](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-service/5-click-create.png)

Kiểm tra:

- **Running tasks** bằng **Desired tasks**.
- Target trong `nexusoms-demo-api-tg` có trạng thái **Healthy**.
- CloudWatch log group `/ecs/nexusoms-demo-api` có log stream mới.
- Task không có public IP.

Nếu target cũ chuyển sang **Draining** trong lúc rollout, chờ deployment hoàn tất rồi kiểm tra target mới.

| Thay đổi | Cách áp dụng |
| --- | --- |
| Sửa giá trị secret | Force new deployment |
| Sửa environment, `ValueFrom` hoặc image | Tạo Task Definition revision mới rồi update service |
| Sửa IAM policy | Xác nhận policy rồi tạo task mới |

Service chỉ hoàn tất khi deployment ở trạng thái **Completed** và target mới **Healthy**. `Running` không thay thế kiểm tra `Healthy`.

## Bước 6: Ghi lại task đang chạy

```powershell
$TaskArn = aws ecs list-tasks --profile nexusoms-demo --region ap-southeast-1 --cluster nexusoms-demo-cluster --service-name nexusoms-demo-api-service --desired-status RUNNING --query "taskArns[0]" --output text
aws ecs describe-tasks --profile nexusoms-demo --region ap-southeast-1 --cluster nexusoms-demo-cluster --tasks $TaskArn --query "tasks[0].{TaskArn:taskArn,TaskDefinition:taskDefinitionArn,StartedAt:startedAt,LastStatus:lastStatus}" --output table
```

Mở task → **Logs** và ghi lại log stream mới nhất:

| Task ID | Task Definition revision | Started at | Log stream | Target |
| --- | --- | --- | --- | --- |
| `<TASK_ID>` | `<REVISION>` | `<STARTED_AT>` | `<LOG_STREAM>` | `Healthy` |

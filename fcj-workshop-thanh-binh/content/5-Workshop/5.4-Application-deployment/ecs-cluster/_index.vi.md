---
title: "ECS cluster"
date: 2026-11-13
weight: 6
chapter: false
---

Tạo ECS cluster để chạy backend bằng AWS Fargate.

## Bước 1: Tạo cluster

Mở **Amazon ECS → Clusters → Create cluster**.

![Mở trang tạo ECS cluster](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/1-open-clusters-dashboard-create-cluster.png)

## Bước 2: Cấu hình cluster

```text
Cluster name: nexusoms-demo-cluster
Infrastructure: AWS Fargate
```

Chọn **Create**.

![Cấu hình ECS cluster](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/2-configure-cluster.png)

## Bước 3: Bật Container Insights

Mở cluster vừa tạo và chọn **Update cluster**.

![Mở phần cập nhật cluster](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/3-choose-update-cluster.png)

Bật **Container Insights**, sau đó lưu thay đổi.

![Bật Container Insights](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/4-turn-on-container-insights.png)

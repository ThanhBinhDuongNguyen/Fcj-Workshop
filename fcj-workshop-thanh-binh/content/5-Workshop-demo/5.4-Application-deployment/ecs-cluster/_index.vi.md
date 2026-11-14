---
title: "ECS cluster"
date: 2026-11-13
weight: 3
chapter: false
---

Tạo ECS cluster dùng AWS Fargate. Container Insights là tùy chọn và có thể phát sinh thêm chi phí.

## 9.1. Cluster

1\. Mở **Amazon ECS → Clusters → Create cluster**.

2\. Cluster name: nexusoms-demo-cluster.

3\. Chọn infrastructure phù hợp Fargate; không cần EC2 capacity provider cho cấu hình này.

4\. Tạo cluster.

5\. Mở cluster → **Configuration**. Xác nhận **Container Insights** đang bật. Nếu chưa, dùng **Update cluster** hoặc **Actions** để bật; Console mới có thể không gọi mục này là "Cluster settings".

## Giá trị cần nhập hoặc sao chép

> Sao chép từng dòng tương ứng vào ô nhập. Các chuỗi nằm trong dấu `<...>` là placeholder và phải được thay bằng giá trị thật của tài khoản hoặc tài nguyên của bạn.

**Amazon ECS → Clusters → Create cluster**

```text
nexusoms-demo-cluster
```

## Thao tác trên AWS Console

> Nếu một tài nguyên phải tạo hai lần, ảnh chỉ minh họa lần đầu. Hãy lặp lại cùng thao tác bằng bộ giá trị thứ hai trong phần hướng dẫn chi tiết.

### Bước 1 - Open Clusters dashboard + Create cluster

Trên AWS Console, thực hiện **Open Clusters dashboard + Create cluster** như hình dưới đây.

**Giá trị dùng ở bước này**

**Amazon ECS → Clusters → Create cluster**

```text
nexusoms-demo-cluster
```

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/1-open-clusters-dashboard-create-cluster.png" title="Open Clusters dashboard + Create cluster" >}}

### Bước 2 - Configure cluster

Trên AWS Console, thực hiện **Configure cluster** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/2-configure-cluster.png" title="Configure cluster" >}}

### Bước 3 - Choose update cluster

Trên AWS Console, thực hiện **Choose update cluster** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/3-choose-update-cluster.png" title="Choose update cluster" >}}

### Bước 4 - Turn on Container Insights

Trên AWS Console, thực hiện **Turn on Container Insights** như hình dưới đây.

{{< figure src="/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/4-turn-on-container-insights.png" title="Turn on Container Insights" >}}

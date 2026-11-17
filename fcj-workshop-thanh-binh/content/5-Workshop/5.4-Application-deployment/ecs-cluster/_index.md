---
title: "ECS cluster"
date: 2026-11-13
weight: 6
chapter: false
---

Create an ECS cluster to run the backend on AWS Fargate.

## Step 1: Create the cluster

Open **Amazon ECS → Clusters → Create cluster**.

![Open the ECS cluster creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/1-open-clusters-dashboard-create-cluster.png)

## Step 2: Configure the cluster

```text
Cluster name: nexusoms-demo-cluster
Infrastructure: AWS Fargate
```

Choose **Create**.

![Configure the ECS cluster](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/2-configure-cluster.png)

## Step 3: Enable Container Insights

Open the new cluster and choose **Update cluster**.

![Open the cluster update page](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/3-choose-update-cluster.png)

Enable **Container Insights**, then save the changes.

![Enable Container Insights](/images/5-Workshop/nexusoms/5.4-Application-deployment/ecs-cluster/4-turn-on-container-insights.png)

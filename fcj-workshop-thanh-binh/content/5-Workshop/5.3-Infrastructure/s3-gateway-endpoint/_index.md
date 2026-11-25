---
title: "S3 Gateway Endpoint"
date: 2026-11-20
weight: 8
chapter: false
---

Create one S3 Gateway Endpoint for both private route tables.

At task startup, Fargate uses this endpoint to download ECR image layers from AWS-managed S3. This is flow 14 in the diagram, separate from the frontend and audit buckets. The backend does not currently store product images, invoices or reports in S3.

## Step 1: Create the endpoint

Open **VPC → Endpoints → Create endpoint**.

![Open the endpoint creation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/1-open-endpoints-dashboard-create-endpoint.png)

## Step 2: Configure the basic settings

```text
Name tag: nexusoms-demo-s3-endpoint
Service category: AWS services
```

![Configure the endpoint](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/2-fill-name-tag-choose-type.png)

## Step 3: Select the S3 service

Search for and select:

```text
com.amazonaws.ap-southeast-1.s3
Type: Gateway
```

![Select the S3 Gateway service](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/3-choose-s3-service.png)

## Step 4: Select the VPC and route tables

```text
VPC: nexusoms-demo-vpc
Route tables: nexusoms-demo-private-a-rt
Route tables: nexusoms-demo-private-b-rt
Policy: Full access
```

A Gateway Endpoint does not use a Security Group.

![Select both private route tables](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/4-choose-2-private-route-tables-full-access-for-policy.png)

## Step 5: Finish

Choose **Create endpoint** and wait for **Available**.

![Create the S3 Gateway Endpoint](/images/5-Workshop/nexusoms/5.3-Infrastructure/s3-gateway-endpoint/5-click-create-endpoint.png)

This endpoint handles S3 traffic. ECR API/registry, SQS, Secrets Manager, CloudWatch Logs and Supabase still use NAT Gateways in the lab configuration.

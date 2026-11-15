---
title: "End-to-end verification"
date: 2026-11-15
weight: 1
chapter: false
---

## 1. Verify AWS resources

```text
ECS service: Running 1/1
Target group: Healthy
CloudFront distribution: Deployed
CloudFront invalidation: Completed
```

## 2. Sign in

Open the CloudFront domain and sign in using the Supabase account.

```text
https://<CLOUDFRONT_DOMAIN>
```

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/1-open-website.png" title="Open NexusOMS" >}}

## 3. Verify the dashboard

Confirm that the dashboard loads data and `/api/*` requests succeed.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/2-checking-overall-dashboard.png" title="Verify the dashboard" >}}

## 4. Verify products and inventory

Create a product, select a warehouse, and enter the initial quantity.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/3-create-product.png" title="Create a product" >}}

## 5. Verify orders

Create an order with the new product. Open the order details and verify the remaining stock.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-checking-order-detail.png" title="Verify order details" >}}

Verify product cost and revenue.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-1-measuring-product-cost.png" title="Verify product cost" >}}

## 6. Verify the webhook

Send a demo webhook and confirm that one order is created.

{{< figure src="/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/5-using-webhook-to-create-order.png" title="Create an order with a webhook" >}}

## 7. Verify CI/CD

Commit a small frontend change and push it to `main`. The workflow must update S3, create a CloudFront invalidation, and keep the ECS target healthy.

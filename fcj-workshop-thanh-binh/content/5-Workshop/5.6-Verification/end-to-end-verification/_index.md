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
SQS main queue: Available 0, In flight 0
SQS dead-letter queue: Available 0
```

## 2. Sign in

Open the CloudFront domain and sign in using the Supabase account.

```text
https://<CLOUDFRONT_DOMAIN>
```

![Open NexusOMS](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/1-open-website.png)

## 3. Verify the dashboard

Confirm that the dashboard loads data and `/api/*` requests succeed.

![Verify the dashboard](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/2-checking-overall-dashboard.png)

## 4. Verify products and inventory

Create a product, select a warehouse, and enter the initial quantity.

![Create a product](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/3-create-product.png)

## 5. Verify orders

Create an order with the new product. Open the order details and verify the remaining stock.

![Verify order details](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-checking-order-detail.png)

Verify product cost and revenue.

![Verify product cost](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-1-measuring-product-cost.png)

## 6. Verify webhook creation and cancellation

Open **Webhook Simulator → Order lifecycle**. Send the create-order webhook and wait for **Processed**.

![Create an order with a webhook](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/5-using-webhook-to-create-order.png)

Send the same `eventId` again and confirm that no second order is created.

Choose an order that has not been dispatched, send an `order.cancelled` event, and confirm:

```text
Order state: Cancelled
Cancellation reason: stored
Reserved stock: released
Available stock: restored once
```

## 7. Verify the SQS burst flow

1. Sign in as a Manager or Admin.
2. Open **Webhook Simulator → Burst demo**.
3. Confirm that the interface shows `SQS` mode.
4. Select a valid channel connection, warehouse, and SKU mapping.
5. Ensure that inventory covers the complete batch quantity.
6. Start a batch of `10` orders.
7. Open **Amazon SQS → nexusoms-demo-webhook-queue.fifo → Monitoring**.
8. Watch available and in-flight messages while the worker processes the batch.
9. Wait until the batch finishes and both values return to `0`.
10. Open CloudWatch Logs and confirm there is no SQS `AccessDenied` error.

While the batch runs, confirm that **Available** or **In flight** increases and then returns to `0`. This demonstrates asynchronous worker processing.

If a message fails three times, inspect `nexusoms-demo-webhook-dlq.fifo` and CloudWatch Logs before redriving or deleting it.

## 8. Finish the demo

Choose **Finish and clean demo data**. Confirm:

```text
No DEMO-* orders remain
Inventory returns to its pre-demo state
The main queue contains no messages
```

If the popup reports a DLQ message, inspect CloudWatch Logs before deleting or redriving it.

## 9. Verify CI/CD

Commit a small frontend change and push it to `main`. The workflow must update S3, create a CloudFront invalidation, and keep the ECS target healthy.

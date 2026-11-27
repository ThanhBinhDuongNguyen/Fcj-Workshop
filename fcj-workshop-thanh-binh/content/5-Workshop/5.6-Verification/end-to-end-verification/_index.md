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

For the two-task configuration shown in the diagram, verify `Running 2/2`, one task in each AZ and both targets `Healthy`.

## 2. Sign in

Open the CloudFront domain and sign in using the Supabase account.

Open DevTools → Network and verify:

```text
Signed out: /api/v1/session/me returns 401
Valid sign-in: /api/v1/session/me returns 200
API domain: <CLOUDFRONT_DOMAIN>
```

```text
https://<CLOUDFRONT_DOMAIN>
```

![Open NexusOMS](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/1-open-website.png)

## 3. Verify the dashboard

Confirm that the dashboard loads data and `/api/*` requests succeed.

Open `/login` and `/app` directly, then refresh. Frontend routes must reload correctly, and API errors must not become HTML responses with HTTP 200.

![Verify the dashboard](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/2-checking-overall-dashboard.png)

## 4. Verify products and inventory

Create a product, select a warehouse, and enter the initial quantity.

Verify data inside NexusOMS. The demo does not publish products or stock changes to live shops.

![Create a product](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/3-create-product.png)

## 5. Verify orders

Open the order list. In **DevTools → Network**, confirm that `/api/v1/orders` returns `200` with JSON data. `/health` and a `Healthy` target do not replace this database check.

Create an order with the new product. Open the order details and verify the remaining stock.

![Verify order details](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-checking-order-detail.png)

Verify product cost and revenue.

![Verify product cost](/images/5-Workshop/nexusoms/5.6-Verification/end-to-end-verification/4-1-measuring-product-cost.png)

## 6. Verify webhook creation and cancellation

Use the built-in simulator to generate events on demand. No orders need to be placed or cancelled on a live marketplace.

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

Create a branch, make a small frontend change, and open a pull request. After CI passes, merge it into `main`. The workflow must update S3, wait for the CloudFront invalidation, and keep the ECS target healthy.

## 10. Quick troubleshooting

| Error | Check |
|---|---|
| `The config profile could not be found` | Run `aws login --profile nexusoms-demo --region ap-southeast-1`, then `aws sts get-caller-identity --profile nexusoms-demo` |
| Docker reports `error getting credentials` | Use the temporary Docker config from the backend image step |
| ECS reports `secretsmanager:GetSecretValue AccessDenied` | Check the execution-role policy, register a new task definition revision, and update the service |
| Target group returns `500` | Open CloudWatch Logs and validate the secret with the PowerShell block in Secrets Manager |
| CloudFront reports `globPattern invalid` | Clear the Path pattern and type `/api/*` again |
| SPA route returns `403` or `404` | Build, Save, and Publish the CloudFront Function; check its Default behavior association |
| `NoSuchDistribution` | Use the Distribution ID, not the CloudFront domain |
| GitHub Actions cannot assume the role | Compare the actual OIDC subject with the role trust policy |
| Two CD workflows are active | Remove `.github/workflows/cd.yml` and keep only `deploy-aws.yml` for AWS deployment |
| A workflow PR does not run CI | Add `.github/workflows/**` to both CI path filters and keep `workflow_call` |

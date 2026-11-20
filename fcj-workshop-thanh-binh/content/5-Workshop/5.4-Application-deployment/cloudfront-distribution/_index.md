---
title: "CloudFront Distribution"
date: 2026-11-13
weight: 11
chapter: false
---

Create one CloudFront Distribution with S3 as the default frontend origin and the ALB as the `/api/*` origin.

## Step 1: Create the distribution

Open **CloudFront → Distributions → Create distribution**.

![Open the distribution creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/1-open-cloudfront-dashboard-create-distribution.png)

Enter the distribution name:

```text
nexusoms-demo-web
```

![Configure the distribution](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/2-configure-distribution-options.png)

## Step 2: Select the frontend S3 origin

Select **Amazon S3**, then select `nexusoms-demo-frontend-<AWS_ACCOUNT_ID>`.

```text
Origin access: Origin access control settings
Viewer protocol policy: Redirect HTTP to HTTPS
Default root object: index.html
```

Keep **Block all public access** enabled on the bucket.

After creating the distribution, use the CloudFront S3 bucket policy prompt and choose **Copy policy** if the Console did not update the OAC permission automatically. Add the policy to the frontend bucket.

![Select the S3 origin](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/3-choose-s3-type-mapping-to-s3-fe.png)

Choose **Next**.

![Continue configuring the distribution](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/4-click-next.png)

## Step 3: Finish the distribution

Under WAF protection, select **Do not enable security protections**.

![Do not enable WAF for the workshop](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/5-choose-do-not-enable-for-demo.png)

Choose **Create distribution** and wait for the status to become **Deployed**.

![Create the distribution](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/6-click-create-distribution.png)

## Step 4: Add the ALB origin

Open the new distribution and choose **Origins → Create origin**.

![Open the origin creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/7-choose-origins-create-origin.png)

```text
Origin domain: <ALB_DNS_NAME>
Origin name: nexusoms-demo-alb-origin
Origin path: leave empty
Protocol: HTTP only
HTTP port: 80
```

Enter only the ALB DNS name in `Origin domain`. Do not include `http://` or `https://`.

![Configure the ALB origin](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/8-configure-setting-for-origin.png)

## Step 5: Create the API behavior

Choose **Behaviors → Create behavior**.

![Open the behavior creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/9-choose-behavior-tab-create-behavior.png)

```text
Path pattern: /api/*
Origin: nexusoms-demo-alb-origin
Viewer protocol policy: Redirect HTTP to HTTPS
Allowed HTTP methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
Cache policy: CachingDisabled
Origin request policy: AllViewerExceptHostHeader
```

![Configure the API behavior](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/10-configure-behavior-setting.png)

![Select the Origin request policy](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-distribution/11-choose-origin-request-policy.png)

Choose **Create behavior**. Verify that `/api/*` points to the ALB origin with caching disabled.

If Path pattern reports `globPattern invalid`, clear the field and type `/api/*` manually.

## Step 6: Record the CloudFront information

Record the **Distribution ID** and **Distribution domain name** for GitHub Actions.

```text
Distribution ID: E...
Distribution domain name: d....cloudfront.net
```

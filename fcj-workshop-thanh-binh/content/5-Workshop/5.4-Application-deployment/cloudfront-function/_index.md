---
title: "CloudFront Function for SPA"
date: 2026-11-13
weight: 12
chapter: false
---

Create a CloudFront Function that serves React routes without file extensions from `/index.html`.

## Step 1: Create the function

Open **CloudFront → Functions → Create function**.

![Open the function creation page](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/1-open-functions-dashboard-create-function.png)

```text
Function name: nexusoms-demo-spa-routing
Runtime: cloudfront-js-2.0
```

![Configure the function](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/2-configure-function-details.png)

## Step 2: Add the SPA rewrite code

Delete the sample code and paste this entire block:

```javascript
function handler(event) {
    var request = event.request;
    var uri = request.uri;

    // Rewrite SPA routes to index.html while preserving static files.
    if (uri === "/" || uri.indexOf(".") === -1) {
        request.uri = "/index.html";
    }

    return request;
}
```

![Paste the code into the Build section](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/3-paste-code-build-section.png)

## Step 3: Publish the function

Choose **Save changes**, then choose **Publish**.

![Publish the function](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/4-publish-function.png)

## Step 4: Associate the function with the distribution

Open the distribution → **Behaviors** → select **Default (*) → Edit**.

![Open the Default behavior](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/5-open-distribution-detail-choose-behaviors-edit.png)

Under **Function associations**, select:

```text
Event type: Viewer request
Function type: CloudFront Functions
Function ARN/Name: nexusoms-demo-spa-routing
Stage: LIVE
```

Do not associate the function with the `/api/*` behavior.

![Associate the function with Viewer request](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/6-configure-function-associations.png)

## Step 5: Verify

Wait for the distribution to become **Deployed**. Copy its domain name and open `/app?page=dashboard`.

![Copy the Distribution domain name](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/7-copy-distribution-domain-name.png)

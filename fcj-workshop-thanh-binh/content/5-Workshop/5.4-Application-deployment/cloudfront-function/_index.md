---
title: "CloudFront Function for SPA"
date: 2026-11-13
weight: 11
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

## Step 2: Open Build and add the SPA rewrite code

Open the function, choose the **Build** tab, delete the sample code, and paste this entire block:

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

## Step 3: Build and publish the function

Follow this order:

1. Choose **Save changes** in the Build tab.
2. Choose **Publish**.
3. Confirm that the function is in the `LIVE` stage.

If Create or Publish reports `UnknownError`, refresh the Functions list and check whether the function was created or published before retrying.

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

Do not configure distribution-wide custom 403/404 responses that return `/index.html` with HTTP 200. The function rewrites only frontend routes on the Default behavior, while API errors keep their original status and body.

![Associate the function with Viewer request](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/6-configure-function-associations.png)

## Step 5: Verify the association

Confirm that the function is **LIVE** and associated with **Viewer request** on **Default (*)**, then wait for the distribution to become **Deployed**. Copy its domain name for [Build and upload the frontend](../frontend-deployment/).

After uploading the frontend and completing the invalidation, open `/app?page=dashboard` to verify SPA routing.

![Copy the Distribution domain name](/images/5-Workshop/nexusoms/5.4-Application-deployment/cloudfront-function/7-copy-distribution-domain-name.png)

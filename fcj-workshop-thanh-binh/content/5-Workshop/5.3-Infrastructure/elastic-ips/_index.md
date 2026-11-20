---
title: "Allocate Elastic IPs"
date: 2026-11-20
weight: 4
chapter: false
---

Allocate two Elastic IPs and record their Allocation IDs for the NAT Gateways.

## Step 1: Allocate Elastic IP A

Open **VPC → Elastic IPs → Allocate Elastic IP address**.

![Open the Elastic IP allocation page](/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/1-open-elastic-ips-allocate-elastic-ip.png)

Keep the default settings, add the tag, and choose **Allocate**:

```text
Key: Name
Value: nexusoms-demo-nat-eip-a
```

![Add the Elastic IP Name tag](/images/5-Workshop/nexusoms/5.3-Infrastructure/elastic-ips/2-fill-the-name-tag-for-elastic-ip-allocate.png)

## Step 2: Allocate Elastic IP B

Repeat with:

```text
Key: Name
Value: nexusoms-demo-nat-eip-b
```

## Step 3: Record the values

| Name | Public IPv4 | Allocation ID |
| --- | --- | --- |
| `nexusoms-demo-nat-eip-a` | `<EIP_A>` | `<ALLOCATION_ID_A>` |
| `nexusoms-demo-nat-eip-b` | `<EIP_B>` | `<ALLOCATION_ID_B>` |

The NAT Gateway dropdown may show only the IP address and Allocation ID. Use this table to select the correct address.

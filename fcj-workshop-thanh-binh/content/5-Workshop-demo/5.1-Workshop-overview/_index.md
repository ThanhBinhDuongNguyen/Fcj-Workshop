---
title: "Workshop overview"
date: 2026-11-13
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

NexusOMS uses CloudFront as the public entry point, S3 for the static frontend, an ALB for API routing, ECS Fargate in private subnets, and Supabase for PostgreSQL and authentication.

## Procedure

Follow the screenshots in order and keep all resource names consistent. The Vietnamese page contains the authoritative field-by-field wording captured from the current AWS Console used for this workshop. AWS Console labels are intentionally kept in English so they can be searched directly.

## AWS Console walkthrough

> When a resource must be created twice, the screenshots show only the first run. Repeat the same action with the second value set documented in the detailed instructions.

### Step 1 - Noxus_AWS_Architect_1

In AWS Console, complete **Noxus_AWS_Architect_1** as shown below.

{{< figure src="/images/5-Workshop/nexusoms/5.1-Workshop-overview/noxus-aws-architect-1.png" title="Noxus_AWS_Architect_1" >}}

---
title: "Event 4"
date: 2026-11-21
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

# Summary Report: “AWS Student Community Day Vietnam 2026”

### Event Objectives

- Bring together the student community passionate about cloud computing and AI in Vietnam.
- Give students direct access to practical knowledge from experts and expand their professional networks in the Cloud & AI era.
- Introduce the AWS Student Builder Groups of HUFLIT, HCMUTE, PTIT and SGU, the four universities that co-organized the event.
- Learn how to protect a cloud system with layered network security on AWS.

### Speakers

- **Kha Van** – Cloud Security Engineer
- **Trung Kiên** – Software Engineer, Core Team, Club HUFLIT
- **Nguyễn Hoàng Phúc** – Core Team Member, AWS Student Builder Group HCMUTE
- **Hồ Đặng Thái Duy** – Core Team Member, AWS Student Builder Group HCMUTE
- **Other speakers** – Core team members of the AWS Student Builder Groups at HUFLIT, HCMUTE, PTIT and SGU

### Key Highlights

#### 1. The AWS Student Builder Community

- **A joint event**: AWS Student Community Day Vietnam 2026 was co-organized by an alliance of AWS Student Builder Groups at HUFLIT, HCMUTE, PTIT and SGU.
- **Community connections**: The event dedicated most of its time to networking and presenting the development journeys of the AWS Student Builder communities at four universities.
- **Journey of Club HUFLIT**: Trung Kiên shared nearly three years-about 1,000 days-of community development.
- **Journey of AWS Student Builder Group HCMUTE**: Two Core Team members shared how the group was established and developed.
- **Communities introduced** (Facebook fanpages): AWS Student Builder Group Saigon University, AWS Student Builder Group at HUFLIT, AWS Student Builder Group PTIT HCM and AWS Student Builder Group HCMUTE.

#### 2. Cloud Network Security (Session by Kha Van)

*A technical session on layered security architecture and connection control on AWS, including network segmentation, traffic control, risk detection and practical implementation.*

- **Why network security on the cloud matters**:
    - Many accounts and VPCs make network boundaries more complex.
    - Access permissions, routing and configuration decide the level of exposure.
    - Overly broad internal connectivity increases the risk of an attack spreading.
    - Resources change quickly, so configurations need regular checks.
    - Encrypted traffic and managed services require suitable monitoring.
- **Reference architecture: protect the system in layers**:
    - *Edge layer*: DDoS protection, website protection and control of access from the Internet.
    - *Network layer*: network segmentation, routing and segmentation policy.
    - *Application layer*: Security Groups, container network policy and identity.
    - *Inspection layer*: inspect the necessary traffic at the network and application layers.
    - *Monitoring layer*: network logs, anomaly detection, connectivity checks and IaC.
- **Network security services available on AWS**:
    - *Security Groups*: define which connections are allowed to and from resources within a VPC.
    - *Network ACLs*: filter traffic at subnet level, with both allow and deny rules.
    - *Route Tables / Transit Gateway (TGW)*: decide paths and connectivity between network zones.
    - *Network Firewall*: inspect traffic, filter domains and detect attacks.
    - *WAF / Shield*: protect web applications and mitigate DDoS attacks.
    - *DNS Firewall*: control domain name resolution requests through DNS.
- **Internal connectivity: stop attacks from spreading between systems**:
    - *Within the same VPC*: use Security Groups to limit connections between EC2 and applications. Traffic inside one VPC does not automatically pass through a central firewall at the TGW.
    - *Between VPCs*: use Transit Gateway and route tables for segmentation, send only the necessary flows through the firewall and check the path in both directions.
    - *Priority*: protect the connections between zones that have different trust levels.

#### 3. Prove It - Agentic Cloud Investigation Series (shown on the slides)

- **Challenge overview**: A challenge series by Cloud Thinker and AWS First Cloud Journey, supported by the University of Greenwich, with two rounds: *Find the Gaps* and *Find the Cause*.
- **Round 1 - evidence rules**: For the question "What would you optimize in this AWS account for cost?", every finding must show the exact tool call, the full raw output, a label (Verified / Inferred / Assumed / Blocked), the source, scope and time, and must not recommend deletion or release as an executable action.
- **Final report in exactly three sections**: The scope reviewed; optimization recommendations ordered by impact and supported by labels, evidence and raw output; and the next steps. Only conclusions supported by sufficient evidence are retained, and all account IDs and ARNs are redacted.
- **Sample scenario**: A static website on AWS shows *Access Denied* while the expected result is that the page opens. The questions to ask are where the request goes, who needs access to what, and whether the proposed fix grants too much access: investigate before changing permissions.
- **Round 2 - find the cause**: Find the fault in Kubernetes infrastructure and propose a solution, for example a container that keeps being OOMKilled and restarting, or an orders-fulfillment pipeline blocked by a JSON error at offset 20.
- **Causal chain**: A chain of logical links from the initial change to the impact on end users: configuration change → technical mechanism → abnormal telemetry → service failure → customer impact.

### Key Takeaways

#### Design Mindset

- **Defense in depth**: Protect the system with several layers (edge, network, application, inspection and monitoring), not with a single control.
- **Exposure is a result of configuration**: Access permissions, routing and configuration decide how exposed a system is, so they must be designed and checked on purpose.
- **Investigate before changing**: Understand where the request goes and who needs access to what before changing permissions.

#### Technical Architecture

- Use **Security Groups** inside a VPC and **Transit Gateway with route tables** between VPCs to segment the network.
- Put **WAF / Shield** at the edge and use **Network Firewall** and **DNS Firewall** to inspect traffic and DNS requests where needed.
- Use **Network ACLs** for subnet-level allow and deny rules.
- Keep **network logs, anomaly detection and IaC** so configurations can be checked regularly.

#### Working Practice

- Prioritize protecting the connections between zones with different trust levels.
- Support every finding with evidence, a label (Verified / Inferred / Assumed / Blocked), source, scope and time.

### Applying to Work

- **Use the five layers as a checklist**: Review the edge, network, application, inspection and monitoring layers of my AWS architectures, such as NexusOMS.
- **Review the network rules**: Check security groups and routing so that only the necessary connections are allowed, especially between public and private subnets.
- **Keep studying security**: Continue studying cloud and AI security in depth, following the questions raised after Cloud Mastery.

### Event Experience

Attending **AWS Student Community Day Vietnam 2026** was an opportunity to meet the AWS Student Builder Groups of four universities. Key experiences included:

#### Learning from the speakers
- Heard how the AWS Student Builder Groups of HUFLIT and HCMUTE grew, including the nearly 3-year journey of Club HUFLIT.
- Followed the session by Kha Van on cloud security, covering the layers of security on the cloud, the difference between production and lab, mechanisms such as security groups and VPC peering, and the main topics of security.

#### Community and networking
- The discussions showed how the four AWS Student Builder Groups organize activities and develop their student communities.
- Learned about the four student communities and their fanpages.

#### Lessons learned
- Security on the cloud is built in layers, from the edge to monitoring, and not with one tool.
- Overly broad internal connectivity makes attacks easier to spread, so connections between zones with different trust levels deserve priority.
- Cloud resources change quickly, so configurations need regular checks.

#### Some event photos
![AWS Student Community Day Vietnam 2026 Attendance Proof](/images/4-Event/aws-student-community-day-2026.jpg)

> Overall, Community Day provided opportunities to connect with the AWS Student Builder Groups and offered practical insights into layered network security architecture on AWS.

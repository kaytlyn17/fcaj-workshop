---
title: "1.4. Week 4 Worklog"


weight: 4
chapter: false
---

05/10/2026 - 09/10/2026

## Objectives

- Understand advanced networking components: Peering, Transit Gateway, VPC Endpoints, ELB.
- Deliver content globally with CloudFront and customize behavior at the edge with Lambda@Edge.
- Get familiar with Windows workloads: managed Active Directory and Amazon WorkSpaces.

## Daily work

| Day | Task | Start | Completion | Lab material |
|---|---|---|---|---|
| Mon | Networking on AWS Workshop (Lab 000092)<br>Region, AZ, Edge Location; VPC Peering, Transit Gateway, Site-to-Site VPN<br>VPC Endpoints (Gateway, Interface), Network Manager, Network Insights<br>ELB: ALB (L7), NLB (L4), CLB | 05/10/2026 | 05/10/2026 | [000092](https://000092.awsstudygroup.com/) |
| Tue | Amazon CloudFront (Lab 000094)<br>Create a distribution, caching, cache invalidation, custom error pages<br>Origin Group (failover), path-based cache behaviors<br>Block public access on S3 and allow only CloudFront (OAC) | 06/10/2026 | 06/10/2026 | [000094](https://000094.awsstudygroup.com/) |
| Wed | CloudFront and Lambda@Edge (Lab 000130)<br>Four trigger points: Viewer Request, Origin Request, Origin Response, Viewer Response<br>Practice URL rewriting and adding security headers<br>Compare Lambda@Edge with CloudFront Functions | 07/10/2026 | 07/10/2026 | [000130](https://000130.awsstudygroup.com/) |
| Thu | Windows Workloads: Amazon WorkSpaces (Lab 000093)<br>Deploy AWS Managed AD and EC2, configure security<br>Deploy WorkSpaces and access it through the browser and the client | 08/10/2026 | 08/10/2026 | [000093](https://000093.awsstudygroup.com/) |
| Fri | AWS Managed Microsoft AD (Lab 000095)<br>Deploy AD, join EC2 to the domain, manage users and groups<br>Weekly review: VPC Peering vs Transit Gateway; Gateway vs Interface Endpoint; ALB vs NLB<br>Clean up resources (WorkSpaces and Directory Service are costly) | 09/10/2026 | 09/10/2026 | [000095](https://000095.awsstudygroup.com/) |

## Achievements

- Understood AWS network connectivity models and can choose between Peering and Transit Gateway based on scale.
- Can distinguish Gateway Endpoints from Interface Endpoints, and ALB from NLB.
- Configured CloudFront with a secured S3 origin and wrote functions that run at the edge.
- Deployed Managed AD, domain join and WorkSpaces.
- Paid attention to the cost of services like Directory Service and WorkSpaces and cleaned up thoroughly.

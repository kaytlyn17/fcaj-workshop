---
title: "1.2. Week 2 Worklog"


weight: 2
chapter: false
---

21/09/2026 - 25/09/2026

## Objectives

- Deploy foundational storage and database services: S3 and RDS.
- Get familiar with Lightsail (VMs and containers) as a simpler alternative to EC2.
- Understand how systems scale and self-heal with EC2 Auto Scaling.

## Daily work

| Day | Task | Start | Completion | Lab material |
|---|---|---|---|---|
| Mon | Amazon S3: static website (Lab 000057)<br>Create a bucket, upload index.html, enable Static website hosting, bucket policy<br>Enable Versioning, try Cross-Region Replication<br>Put CloudFront in front of S3 and block direct public access | 21/09/2026 | 21/09/2026 | [000057](https://000057.awsstudygroup.com/) |
| Tue | Amazon RDS (Lab 000005)<br>Create a DB Subnet Group and Security Group, launch RDS<br>Connect from EC2 and deploy an application that uses the database<br>Backup, snapshot, restore; distinguish Multi-AZ from Read Replica | 22/09/2026 | 22/09/2026 | [000005](https://000005.awsstudygroup.com/) |
| Wed | Amazon Lightsail (Lab 000045)<br>Deploy a database and WordPress, create a snapshot, upgrade the plan, create an alarm<br>Lightsail Containers (Lab 000046)<br>Create a container service, deploy a public image, build and push a custom image | 23/09/2026 | 23/09/2026 | [000045](https://000045.awsstudygroup.com/) · [000046](https://000046.awsstudygroup.com/) |
| Thu | EC2 Auto Scaling (Lab 000006)<br>Create a Launch Template, Target Group and Application Load Balancer<br>Create an Auto Scaling Group and a scaling policy (target tracking)<br>Simulate load to verify scale out and scale in | 24/09/2026 | 24/09/2026 | [000006](https://000006.awsstudygroup.com/) |
| Fri | Compare services: Lightsail vs EC2; S3 vs EBS vs EFS; Multi-AZ vs Read Replica<br>Draw architecture diagrams of this week&#x27;s labs with draw.io<br>Clean up resources and check Billing | 25/09/2026 | 25/09/2026 | [cloudjourney](https://cloudjourney.awsstudygroup.com/) |

## Achievements

- Hosted a static website on S3 and learned how to protect it with CloudFront.
- Deployed RDS, connected to it from an EC2 application and practiced backup/restore.
- Can tell when to use Lightsail and when to use EC2.
- Configured an ALB + Auto Scaling Group architecture and observed the system scaling automatically.
- Learned to draw AWS architecture diagrams with draw.io.

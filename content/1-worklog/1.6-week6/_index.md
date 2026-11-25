---
title: "1.6. Week 6 Worklog"


weight: 6
chapter: false
---

19/10/2026 - 23/10/2026

## Objectives

- Automate operations with Lambda and Systems Manager.
- Monitor with CloudWatch advanced features and Grafana.
- Govern resources and control permissions with tags.

## Daily work

| Day | Task | Start | Completion | Lab material |
|---|---|---|---|---|
| Mon | Serverless Automation with Lambda (Lab 000022)<br>Tag instances and create an IAM Role for Lambda<br>Write two functions to start/stop EC2 on a schedule (EventBridge) with Slack notifications | 19/10/2026 | 19/10/2026 | [000022](https://000022.awsstudygroup.com/) |
| Tue | Grafana (Lab 000029): install Grafana on EC2 and monitor<br>CloudWatch Advanced (Lab 000036)<br>Search/math expressions, dynamic labels, Container Insights, reducing MTTR | 20/10/2026 | 20/10/2026 | [000029](https://000029.awsstudygroup.com/) · [000036](https://000036.awsstudygroup.com/) |
| Wed | Tags and Resource Groups (Lab 000027)<br>Tag-based access control with IAM (Lab 000028)<br>Policy conditions, Switch Role, testing tagged and untagged EC2 instances | 21/10/2026 | 21/10/2026 | [000027](https://000027.awsstudygroup.com/) · [000028](https://000028.awsstudygroup.com/) |
| Thu | Systems Manager: Patch Manager and Run Command (Lab 000031)<br>Attach an IAM Role for SSM, patch servers in bulk, run commands on many servers | 22/10/2026 | 22/10/2026 | [000031](https://000031.awsstudygroup.com/) |
| Fri | Session Manager (Lab 000058)<br>Connect to private instances without opening SSH/RDP or using a bastion host<br>Create 3 VPC Endpoints: ssm, ssmmessages, ec2messages; store session logs in S3<br>Weekly review: Run Command vs Session Manager vs Patch Manager; clean up | 23/10/2026 | 23/10/2026 | [000058](https://000058.awsstudygroup.com/) |

## Achievements

- Automatically started/stopped EC2 on a schedule with Lambda to save costs.
- Built Grafana dashboards and queried logs with CloudWatch Logs Insights.
- Used tags to organize resources and applied fine-grained IAM conditions.
- Managed servers without opening SSH/RDP ports thanks to Session Manager.
- Understood the role of each Systems Manager capability.

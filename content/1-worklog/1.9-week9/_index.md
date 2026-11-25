---
title: "1.9. Week 9 Worklog"


weight: 9
chapter: false
---

09/11/2026 - 13/11/2026

## Objectives

- Detect threats and manage secrets securely.
- Build backup, recovery and reliable network connectivity.
- Explore high availability patterns for databases.

## Daily work

| Day | Task | Start | Completion | Lab material |
|---|---|---|---|---|
| Mon | AWS Secrets Manager (Lab 000096): store RDS credentials, Secret Rotation, access from Fargate<br>Amazon Cognito (Lab 000141)<br>Distinguish User Pools (authentication) from Identity Pools (AWS credentials) | 09/11/2026 | 09/11/2026 | [000096](https://000096.awsfcaj.com/) · [000141](https://000141.awsfcaj.com/) |
| Tue | Amazon GuardDuty (Lab 000098)<br>Simulate a compromised EC2 instance and leaked credentials<br>Automated remediation: GuardDuty, EventBridge, Lambda, SNS<br>AWS Firewall Manager (Lab 000097): centralized Security Group management | 10/11/2026 | 10/11/2026 | [000098](https://000098.awsfcaj.com/) · [000097](https://000097.awsfcaj.com/) |
| Wed | AWS Backup (Lab 000013): create a Backup Plan, SNS notifications, test restore<br>EBS Data Lifecycle Manager (Lab 000088)<br>AWS Backup Anomaly Detection for EBS (Lab 000089) | 11/11/2026 | 11/11/2026 | [000013](https://000013.awsfcaj.com/) · [000088](https://000088.awsfcaj.com/) · [000089](https://000089.awsfcaj.com/) |
| Thu | VPC Peering (Lab 000019)<br>Create a Peering Connection, update both route tables and NACLs, cross-peer DNS<br>AWS Transit Gateway (Lab 000020)<br>Create a TGW, TGW route tables and VPC attachments | 12/11/2026 | 12/11/2026 | [000019](https://000019.awsfcaj.com/) · [000020](https://000020.awsfcaj.com/) |
| Fri | SNS and SQS (Lab 000077): pub/sub and message filtering<br>Study high availability: EBS Multi-Attach + NVMe Reservation, Windows Server Failover Clustering, SQL Server HA (Labs 100000, 100001, 100002, 100003, 100004)<br>EC2 Image Builder + Systems Manager automated patching (Lab 000099); clean up | 13/11/2026 | 13/11/2026 | [000077](https://000077.awsfcaj.com/) · [100000](https://100000.awsfcaj.com/) · [100001](https://100001.awsfcaj.com/) · [100002](https://100002.awsfcaj.com/) · [100003](https://100003.awsfcaj.com/) · [100004](https://100004.awsfcaj.com/) · [000099](https://000099.awsfcaj.com/) |

## Achievements

- Managed credentials securely and rotated them automatically with Secrets Manager.
- Built a detect-and-remediate flow with GuardDuty, EventBridge and Lambda.
- Set up automated backups, tested restores and detected anomalies in backups.
- Can choose between VPC Peering and Transit Gateway; decoupled systems with SNS/SQS.
- Understood database HA patterns: Multi-Attach, WSFC and SQL Server FCI.

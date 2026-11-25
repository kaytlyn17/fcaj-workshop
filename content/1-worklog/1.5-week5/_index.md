---
title: "1.5. Week 5 Worklog"


weight: 5
chapter: false
---

12/10/2026 - 16/10/2026

## Objectives

- Learn the main methods to migrate servers and databases to AWS.
- Understand a low-cost Disaster Recovery solution.
- Get familiar with hybrid storage between on-premises and AWS.

## Daily work

| Day | Task | Start | Completion | Lab material |
|---|---|---|---|---|
| Mon | VM Import/Export (Lab 000014)<br>Prepare the source VM, export the virtual disk and upload it to S3<br>Import it as an AMI, launch EC2; export back to on-premises<br>Configure the vmimport IAM role | 12/10/2026 | 12/10/2026 | [000014](https://000014.awsfcaj.com/) |
| Tue | Database Migration: DMS and SCT (Lab 000043), part 1<br>Install SCT and configure the source (Oracle/SQL Server)<br>Schema conversion to the target engine | 13/10/2026 | 13/10/2026 | [000043](https://000043.awsfcaj.com/) |
| Wed | Database Migration: DMS and SCT (Lab 000043), part 2<br>Create a replication instance, endpoints and a migration task<br>Replicate changes (CDC), monitor with CloudWatch, troubleshoot<br>Rule of thumb: different engines mean SCT first, then DMS | 14/10/2026 | 14/10/2026 | [000043](https://000043.awsfcaj.com/) |
| Thu | AWS Elastic Disaster Recovery - DRS (Lab 000100)<br>Build a simulated on-premises environment and install the DRS Agent<br>Configure the EC2 Launch Template and perform a failover | 15/10/2026 | 15/10/2026 | [000100](https://000100.awsfcaj.com/) |
| Fri | Storage Gateway (Lab 000024): File Gateway sharing files between on-premises and S3<br>Amazon FSx for Windows File Server (Lab 000025)<br>Storage Performance (Lab 000068): S3 and EFS performance<br>Compare DMS, SCT, VM Import/Export, DRS and MGN; clean up resources | 16/10/2026 | 16/10/2026 | [000024](https://000024.awsfcaj.com/) · [000025](https://000025.awsfcaj.com/) · [000068](https://000068.awsfcaj.com/) |

## Achievements

- Can pick the right migration tool: VM Import/Export, DMS + SCT, DRS or MGN.
- Practiced heterogeneous database migration with continuous replication (CDC).
- Understood the DR model: continuous replication, low cost, fast failover.
- Deployed File Gateway and compared file storage options (EFS, FSx, S3).
- Learned what drives S3 and EFS performance.

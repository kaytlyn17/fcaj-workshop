---
title: "1.3. Week 3 Worklog"


weight: 3
chapter: false
---

28/09/2026 - 02/10/2026

## Objectives

- Learn centralized system monitoring with CloudWatch.
- Understand DNS on AWS and the Hybrid DNS model with Route 53 Resolver.
- Get familiar with the AWS CLI and NoSQL/caching services: DynamoDB and ElastiCache.

## Daily work

| Day | Task | Start | Completion | Lab material |
|---|---|---|---|---|
| Mon | Amazon CloudWatch (Lab 000008)<br>Metrics, Logs, Alarms, Dashboards<br>CloudWatch Logs Insights, Metric Filters<br>Create an alarm that sends notifications through SNS | 28/09/2026 | 28/09/2026 | [000008](https://000008.awsfcaj.com/) |
| Tue | Amazon Route 53: Hybrid DNS (Lab 000010)<br>Set up Microsoft AD, create an Outbound Endpoint and Resolver Rules<br>Create an Inbound Endpoint and test name resolution between AWS and on-premises | 29/09/2026 | 29/09/2026 | [000010](https://000010.awsfcaj.com/) |
| Wed | AWS CLI (Lab 000011)<br>Configure named profiles with aws configure<br>Work with S3, SNS, IAM, VPC and EC2 from the CLI<br>Explore output formats: json, yaml, text, table | 30/09/2026 | 30/09/2026 | [000011](https://000011.awsfcaj.com/) |
| Thu | Amazon DynamoDB (Lab 000060)<br>Partition Key, Sort Key, LSI, GSI, consistency, capacity modes<br>Put, Get, Update, Delete, Query, Scan and Batch operations with the AWS SDK (Python) | 01/10/2026 | 01/10/2026 | [000060](https://000060.awsfcaj.com/) |
| Fri | Amazon ElastiCache (Lab 000061)<br>Create a subnet group and a Redis cluster; connect, set/get, pub/sub, streams<br>Compare Cluster Mode Disabled and Enabled<br>Weekly review: compare RDS, DynamoDB, ElastiCache and Redshift; clean up resources | 02/10/2026 | 02/10/2026 | [000061](https://000061.awsfcaj.com/) |

## Achievements

- Built dashboards, metric filters and alarms with CloudWatch.
- Understood and configured Hybrid DNS with Route 53 Resolver (Inbound and Outbound Endpoints).
- Used the AWS CLI with multiple profiles to manage resources instead of the console.
- Designed DynamoDB tables with suitable keys and indexes and operated them through the SDK.
- Learned to choose the right database for each use case.

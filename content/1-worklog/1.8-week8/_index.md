---
title: "1.8. Week 8 Worklog"


weight: 8
chapter: false
---

02/11/2026 - 06/11/2026

## Objectives

- Manage identity and permissions at enterprise level (Identity Center, Permission Boundary).
- Protect data and applications with WAF, KMS, Macie and S3 best practices.
- Understand private access to AWS services through VPC Endpoints.

## Daily work

| Day | Task | Start | Completion | Lab material |
|---|---|---|---|---|
| Mon | IAM Identity Center (Lab 000012)<br>Create accounts in Organizations, build OUs and permission sets<br>Access the AWS CLI with SSO, time-based access control | 02/11/2026 | 02/11/2026 | [000012](https://000012.awsstudygroup.com/) |
| Tue | IAM Permission Boundary (Lab 000030)<br>Create a Restriction Policy and an IAM Limited User, test the limits<br>IAM Role and Condition (Lab 000044)<br>Restrict Switch Role by IP address and time | 03/11/2026 | 03/11/2026 | [000030](https://000030.awsstudygroup.com/) · [000044](https://000044.awsstudygroup.com/) |
| Wed | AWS Security Hub (Lab 000018): enable Security Standards and review the Security Score<br>Private access to S3 through VPC Endpoints (Lab 000111)<br>Gateway Endpoint, Interface Endpoint, on-premises DNS simulation, Endpoint Policies | 04/11/2026 | 04/11/2026 | [000018](https://000018.awsstudygroup.com/) · [000111](https://000111.awsstudygroup.com/) |
| Thu | AWS WAF (Lab 000026)<br>Web ACL, managed rules, custom rules, logging<br>AWS KMS (Lab 000033)<br>Encrypt S3 with KMS, log with CloudTrail, query with Athena | 05/11/2026 | 05/11/2026 | [000026](https://000026.awsstudygroup.com/) · [000033](https://000033.awsstudygroup.com/) |
| Fri | Amazon Macie (Lab 000090): Custom Data Identifier, Macie job, review findings<br>S3 Security Best Practices (Lab 000069)<br>Enforce HTTPS and SSE-S3, S3 Block Public Access, AWS Config, IAM Access Analyzer<br>Clean up resources | 06/11/2026 | 06/11/2026 | [000090](https://000090.awsstudygroup.com/) · [000069](https://000069.awsstudygroup.com/) |

## Achievements

- Managed centralized sign-in for multiple accounts with permission sets.
- Understood and applied Permission Boundaries to prevent privilege escalation.
- Accessed S3 fully privately through VPC Endpoints; distinguished Gateway and Interface Endpoints.
- Protected web apps with WAF, encrypted data with KMS and discovered sensitive data with Macie.
- Locked down S3 following best practices.

---
title: "5.8 · Clean up resources"
weight: 8
chapter: false
aliases:
  - /5-workshop/09-demo-cleanup/
---

1. Begin cleanup only when no one still needs the demo environment. Export test results, the estimate and billing data. Inventory resources in the application stack, KB stack and outside CloudFormation.
2. Stop new submissions, review executions waiting for approval and handle them according to the lab's policy. Remove only the lab's SES receipt rule. If DNS or a domain is shared, preserve MX records and rules used by other email workloads.
3. Remove CloudFront resource associations as appropriate, disable the distribution, wait for **Deployed**, then delete it when ready. Check any flat-rate subscription, standalone WAF/web ACL and recurring charges. Unsubscribe demo email addresses if the SNS topic is retained.
4. Delete data sources and the KB before their vector-storage resources. Review resources with `Retain` or `RetainExceptOnCreate`, including tables, buckets, keys and the user pool. For versioned S3 buckets, check versions and delete markers. Keep evidence for its committed retention period, then remove it using the agreed lab procedure.
5. Review CloudFormation, S3, S3 Vectors, Bedrock Guardrails, IAM roles, Cognito domains, CloudWatch logs/alarms, SQS, SNS, Route 53, Budgets and any dedicated SAM artifact bucket. Schedule KMS key deletion only after identifying data that still needs decryption. Recheck Bills/Cost Explorer after data updates, including automatic domain renewal if a domain was purchased for the lab.

For the separate edge stack, disassociate the web ACL from its distribution before deleting the ACL and stack. Record each retained resource, its owner and the next date for reviewing its charges.

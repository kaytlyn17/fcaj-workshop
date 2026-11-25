---
title: "Is S3 data safe once Block Public Access is enabled?"
weight: 1
chapter: false
menuTitle: "3.1. Blog 1"
---

A team keeps its reports in a private S3 bucket. One of its applications has permission to read them. If someone obtains that application's credentials, what would let the team discover which reports were read? I used this question to look at three different jobs in S3 security.

1. **Limit access.** S3 Block Public Access guards against public access granted by policies and ACLs. A role with effective permission to call `s3:GetObject` can still read objects in scope. I would start by listing the prefixes each role needs to read and checking whether its policy grants access beyond them.
2. **Record object activity.** I would enable CloudTrail S3 data events for reads on the reports bucket. This makes it possible to look up the principal, time and object associated with a `GetObject` request. A trail records these object-level events when you explicitly select them.
3. **Notify someone who can investigate.** GuardDuty S3 Protection can flag unusual access. An EventBridge rule can route relevant findings to an SNS topic with a confirmed email subscription. GuardDuty can analyse S3 activity independently of the trail I configure for my own audit records.

To exercise the notification path, I would use a GuardDuty sample finding that matches the rule and check whether the email arrives. That verifies delivery. To investigate a real alert, I would then consult the object-level history for the affected bucket.

## AWS documentation

- [S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html)
- [CloudTrail data events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html)
- [GuardDuty S3 Protection](https://docs.aws.amazon.com/guardduty/latest/ug/s3-protection.html)


![Blog Picture]({{< asset "images/blogposted_1.png" >}})

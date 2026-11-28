---
title: "5.6 · Well-Architected review"
weight: 6
chapter: false
---

This section reviews how PhishGuard is operated, the reasons behind its design choices and the remaining limitations. During an investigation, the portal incident, its Step Functions execution and the S3 evidence version provide complementary views. The incident ID connects them.

## Comparing the architecture with the configuration

CloudFront serves the portal from S3 and routes `/api/*` to the HTTP API. WAF is associated with the distribution. Web and SES submissions converge at Ingest, which stores the original evidence before starting the workflow. Lambda uses AWS-managed networking; the template defines no application subnets.

Within the workflow, `PrepareAndWaitApproval` is a Task using `lambda:invoke.waitForTaskToken`. PrepareApproval stores the token, sends the notification and returns. The Task continues waiting for a callback, with a default timeout of 48 hours. The Prepare approval and Wait for approval boxes in the diagram describe two parts of this same mechanism.

## AWS Well-Architected design decisions

| Pillar | Application in PhishGuard | Results and limitations |
|---|---|---|
| Operational Excellence | SAM deploys the backend; change sets expose the impact of an update. Two alarms monitor workflow failures and messages in the DLQ, with SNS notification actions. | The image in [5.5.1](../verify/account/) shows OK alarms with actions enabled at capture time. An actual alarm transition and its corresponding notification have not been demonstrated. |
| Security | JWTs and Cognito groups control API access. Raw evidence is isolated, with SSE-KMS, versioning and seven-day Governance retention. Admin API checks permissions before issuing raw access or accepting a decision. | H02/H15 describe authorization checks. An appropriately authorized identity can bypass Governance. Pattern-based PII masking can miss names, addresses and OCR errors. |
| Reliability | Conditional writes and incident-named executions support retries. Asynchronous Ingest has a DLQ. The workflow has an approval timeout, and synthesis has a fallback. | H04 and the partial-failure portion of H13 use local simulation. H14 has an expired-approval screenshot and a 409 response. Region-wide recovery objectives, RTO and RPO, have not been measured. |
| Performance Efficiency | Direct S3 uploads reduce data passing through the API. ExtractRedact uses x86_64, 512 MiB and OCR/QR layers. The callback does not keep a Lambda invocation running while a person decides. | No p50/p95 benchmark is available. Measure image and text workloads separately using Duration, Billed Duration, Max Memory Used and Throttles. |
| Cost Optimization | Estimates use report counts, image volume, tokens and fixed service charges. Intake has a lifecycle policy, and logs have retention settings. | [5.7](../cost/) separates the budget, Calculator export and Cost Explorer data. WAF dominates the low-volume estimate; actual project-specific charges remain incompletely reconciled. |
| Sustainability | Event-driven processing avoids an application server waiting continuously for work. Temporary data has a retention limit. | Cleanup must also account for old object versions. The project has no energy-use or emissions measurement. |

## Handling a message in the DLQ

1. Identify the alarm time and find the Ingest failure in CloudWatch Logs.
2. Check that the input object still exists, verify read/write permissions and inspect any saved evidence version.
3. Look up the DynamoDB incident and the execution with the same name to determine whether the workflow has started.
4. Fix the cause and retry with lab data. Compare incident counts, raw versions and executions to detect duplicate processing.

Intake has a three-day lifecycle. A message left in the DLQ for longer may reference an expired input object. Determine which data can still be recovered before replaying the event.

## Protection boundaries of the lab

WAF filters requests through CloudFront. Direct S3 uploads are controlled by the presigned policy and Ingest; incoming email passes through SES. The template retains the HTTP API's `execute-api` endpoint and does not require every API call to pass through CloudFront. JWT checks, authorization and API throttling still apply to direct calls.

The Cognito template sets MFA to OFF. Raw-evidence auditing records issuance of a 60-second URL, rather than proving an object download. These areas need further assessment before using real data or deploying for an organization. Containment currently simulates an action or updates an internal list; it does not modify a firewall or email gateway.

[AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/the-pillars-of-the-framework.html)

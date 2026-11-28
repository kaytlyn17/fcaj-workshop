---
title: "5.5.1 · Account and monitoring"
weight: 1
chapter: false
aliases:
  - /5-workshop/06-audit/
---

## Demo account snapshot

At **21:16 on November 20, 2026, Vietnam time** (14:16 UTC), the `phishguard-ai` stack in `us-east-1` was `UPDATE_COMPLETE`. Its configured timeout was **172800 seconds (48 hours)**, containment mode **DRY_RUN**, and model `amazon.nova-lite-v1:0`. The main stack inventory contained eight Lambda functions, five S3 buckets, three DynamoDB tables and two alarms.

The tool read all four web-bucket public-access-block settings, POST CORS for the configured origin, and evidence versioning/SSE-KMS/seven-day Governance retention. ExtractRedact used the OCR layer, 512 MiB and x86_64. SNS had one confirmed email subscription. Both alarms were OK, with actions enabled and directed to SecOps.

The November 20 inventory did not fully verify CloudFront behaviors, OAC, Web ACL and plan; the active SES receipt rule; Bedrock KB/Guardrail configuration; or Budgets. `waf_parameter_only=false` reflects a stack parameter, not whether CloudFront has a web ACL attached. Resources configured outside the stack require console checks using an identity with read access. Updates from November 23–24 are listed separately at the end of this page.

## Run the configuration inventory

An existing project environment can reuse its `.venv` with the extracted `tools/audit_phishguard.py`. Run the example below from the package's `lab/` directory. The tool reads configuration and writes a local JSON file. It reports the count of confirmed email subscriptions without exporting their addresses. It does not call Cost Explorer, read report contents or export task tokens and bills.

```powershell
$recipient = Read-Host 'SES lab recipient'
.\.venv\Scripts\python.exe ..\tools\audit_phishguard.py --profile $pgProfile --region us-east-1 --stack phishguard-ai --recipient $recipient --output .\phishguard-account.json
```

Replace `--recipient` with the receiving address of your lab. If the stack does not store a distribution ID, add `--distribution-id` for the distribution serving the portal. Use `--budget-name` when an existing budget name does not contain “PhishGuard”.

## Check the console configuration

| AWS Console location | Configuration to check | Data to compare |
|---|---|---|
| **CloudFormation → phishguard-ai → Outputs / Parameters / Resources** | Account/region, bucket names, HTTP API, dashboard URL, KB, model, `ContainmentMode` and approval timeout. Compare resources in the application and KB stacks. | Inventory JSON and the corresponding parameters. |
| **CloudFront → portal distribution → General / Security / Behaviors** | Distribution domain matches the dashboard URL. Check its PAYG or Free/Pro/Business/Premium plan, the actual web ACL association and CloudFront/global scope, and the managed/rate rules from 5.3.3. `EnableWaf` is only a SAM parameter. | Plan, attached ACL, rule count and observation time. An unattached ACL should be recorded as such. |
| **S3 → web / intake / evidence** | Block Public Access, OAC, Intake CORS for the portal origin, notifications restricted to `uploads/`, three-day expiration for `uploads/` and `inbound/`, and evidence versioning, seven-day GOVERNANCE retention and SSE-KMS. | Bucket properties and the raw `Version ID` referenced by an incident. |
| **SES, SNS, Cognito, Bedrock** | Active receipt rule; confirmed SNS email subscription; correct app client, callbacks and groups; active KB with a completed sync; configured model/profile and published Guardrail version. | An SES incident ID, retrieved playbook sources and the observed Guardrail result. |
| **CloudWatch → Alarms, Metrics** | Both alarm actions target the SecOps topic. The screenshot below records their state at capture time. Actual delivery requires a matching SNS email or alarm history showing a transition. | Alarm state, the H06 `PhishGuard / UnverifiedSender` metric and timestamped logs. |
| **Billing and Cost Management → Budgets** | Monthly cost budget, scope and notification recipient. Example thresholds are 80% actual and 100% forecast against a budget appropriate for the estimate. | Budget name, thresholds and notification settings. A budget alert does not stop charges. |

![PhishGuard-Workflow-Failed and PhishGuard-Ingest-DLQ-NotEmpty show OK with actions enabled.]({{< asset "images/phishguard/alarms.png" >}})

*The screenshot confirms the two alarms' states and enabled actions at capture time.*

If the account has no budget, use **Budgets → Create budget → Customize → Cost budget**. Select Monthly, enter the estimate including domain costs, and add thresholds and the responsible person's email. Check the account/tag scope after saving. [AWS Budgets pricing](https://aws.amazon.com/aws-cost-management/aws-budgets/pricing/)

In the JSON, `MATCH` means the observed value satisfies the tool's check; `REVIEW` calls for further inspection; `UNVERIFIED` commonly means a permission, API or identifier was unavailable; `MANUAL` requires a console check; and `OBSERVED` reports an inventory value. If a read returns `AccessDenied`, an authorized reader can check the resource. Resources configured separately, including SES, WAF and CloudFront, may be absent from the SAM stack's Resources list.

## Updates from November 23–24

The separate web ACL was associated, and the probe returned 200/403. After disabling the probe, GETs with and without its header both returned 200. The KB sync scanned five files, added two and reported zero failures. The portal recorded the four image cases in 5.5.3. These observations supplement the November 20 inventory; they do not alter the AccessDenied result recorded by the earlier API check.

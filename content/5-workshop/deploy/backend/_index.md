---
title: "5.3.1 · Backend and image processing"
weight: 1
chapter: false
aliases:
  - /5-workshop/03-backend/
---

1. Open `template.yaml`. Check the region, eight Lambda functions, three on-demand DynamoDB tables with PITR, five regular S3 buckets, KMS, a Standard workflow, SNS and the SQS DLQ. `layers/ocr-vie-py312-x86_64.zip` is attached only to ExtractRedact. S3 Vectors, the CloudFront distribution, SES receipt rule, sign-in domain and Guardrail are configured later.
2. In a **new lab account**, build and create the initial stack. Enter the email domain of registered reporters, such as your group's internal email domain. The receiving address is configured separately in SES. `--resolve-s3` lets SAM use a packaging bucket before the application's Intake bucket exists.

```powershell
$pgSenderDomain = Read-Host 'Mien email cua reporter, vi du congty.vn'
sam validate --lint --template-file template.yaml --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Template chua hop le.' }
sam build --template-file template.yaml
if ($LASTEXITCODE -ne 0) { throw 'Build that bai.' }
sam deploy --template-file .aws-sam/build/template.yaml --stack-name $pgStack --region $pgRegion --profile $pgProfile --resolve-s3 --capabilities CAPABILITY_NAMED_IAM --parameter-overrides "AllowedSenderDomains=$pgSenderDomain" "ContainmentMode=DRY_RUN" --confirm-changeset
if ($LASTEXITCODE -ne 0) { throw 'Kiem tra su kien CloudFormation.' }
```

If local Python lacks a native dependency compatible with Lambda, start Docker, run `sam build --use-container` and deploy that build. Review the change set before confirming. Resource creation is expected for a new stack. For a stack containing data, inspect any `Remove` or `Replacement=True` entry affecting a table or bucket.

![A backend update change set with Operation and Replacement columns.]({{< asset "images/phishguard/sam-changeset.png" >}})

*This is an update to the demo stack. A new deployment shows Add operations; the image illustrates where to inspect the impact.*

3. Read the stack Outputs. Keep this helper available for subsequent steps.

```powershell
function Get-PgStack {
  $raw = aws cloudformation describe-stacks --stack-name $pgStack --region $pgRegion --profile $pgProfile --output json --no-cli-pager
  if ($LASTEXITCODE -ne 0) { throw 'Khong doc duoc stack.' }
  (($raw -join "`n") | ConvertFrom-Json).Stacks[0]
}
$pg = Get-PgStack
$pgOut = @{}
foreach ($o in $pg.Outputs) { $pgOut[$o.OutputKey] = $o.OutputValue }
$pg.StackStatus
$pgOut['WebBucketName']
([uri]$pgOut['HttpApiEndpoint']).Host
```

**Check before continuing.** CloudFormation should show `CREATE_COMPLETE` or `UPDATE_COMPLETE`. Use the web bucket, HTTP API hostname, UserPoolId, UserPoolClientId and SecOpsTopicArn returned by your own stack.

### Check both Lambda layers

```powershell
aws lambda get-function-configuration --function-name PhishGuard-ExtractRedact --region $pgRegion --profile $pgProfile --query "{Memory:MemorySize,Architecture:Architectures,Layers:Layers[*].Arn}"
```

Verify the OCR and QR layer versions, 512 MiB memory and x86_64 architecture. If a layer cannot be published, inspect the first CREATE_FAILED event. Use the denied action and resource to check the deploying identity's permissions before retrying. Section 5.5.3 tests decoding after the portal is available.

### Updating an existing stack

Build the release source. Inspect `Get-PgStack` and use `Update-PgStack @{}` from [5.3.2](../portal/) to reuse its parameters. Inspect each change before execution. This release updates shared Lambda code. Stop for review if a change removes or replaces a data resource. Wait for `UPDATE_COMPLETE` and verify the indexed status of [playbook 06](../../integrations/knowledge-base/).

---
title: "5.3.2 · Portal and access control"
weight: 2
chapter: false
aliases:
  - /5-workshop/04-portal/
---

1. In **Cognito → User pools → PhishGuard-Users → Domain**, choose an available prefix in `us-east-1` and note the full domain. Use the public `PhishGuard-Web` app client with authorization code flow and PKCE. Classic hosted UI also works when the domain is created through the CLI. For managed login v2, assign branding to the app client.
2. In **CloudFront**, create or open the portal distribution. Use the bucket name and API hostname from stack Outputs. If two origins point to S3, select the origin with **Origin access control** and check the target selected by the behavior. The API needs one separate origin. Set the default root object to `index.html`.

| Behavior | Origin / protocol | Cache and headers |
|---|---|---|
| `Default (*)` | Web bucket's S3 REST endpoint, OAC signing, redirect HTTP to HTTPS | Cache static assets. For deep links, publish `frontend/deploy/cloudfront-spa-router.js` as a CloudFront Function and associate it with **viewer-request** on this behavior only. |
| `/api/*` | `…execute-api.us-east-1.amazonaws.com`, HTTPS only, empty Origin path for the `$default` stage | Allow all methods in the preset to include POST. Select **CachingDisabled** and **AllViewerExceptHostHeader** so the browser's JWT reaches the HTTP API. Put this behavior before the default. |

![Cognito domain-prefix field in us-east-1.]({{< asset "images/phishguard/cognito-domain.png" >}})

*Enter the prefix here, then use the complete Cognito hostname when building the frontend.*

3. Note the distribution ID and `d….cloudfront.net` domain. The following helper updates SAM for the portal and later Bedrock steps. It reads the deployed parameters, then replaces only the requested values. For an existing stack, first verify that the source you are deploying matches that stack.

```powershell
function Update-PgStack([hashtable] $changes) {
  $all = @{}
  foreach ($p in (Get-PgStack).Parameters) {
    if ($null -ne $p.ParameterValue -and "$($p.ParameterValue)" -ne '') {
      $all[$p.ParameterKey] = [string]$p.ParameterValue
    }
  }
  foreach ($name in $changes.Keys) { $all[$name] = [string]$changes[$name] }
  $overrides = @(foreach ($name in $all.Keys) { "$name=$($all[$name])" })
  sam deploy --template-file .aws-sam/build/template.yaml --stack-name $pgStack --region $pgRegion --profile $pgProfile --resolve-s3 --capabilities CAPABILITY_NAMED_IAM --parameter-overrides @overrides --confirm-changeset --no-fail-on-empty-changeset
  if ($LASTEXITCODE -ne 0) { throw 'Kiem tra change set va su kien stack.' }
}
$pgDist = (Read-Host 'CloudFront distribution ID').Trim()
$pgSite = 'https://' + (Read-Host 'Chi nhap hostname CloudFront').Trim().TrimEnd('/')
Update-PgStack @{CloudFrontDistributionId=$pgDist; CloudFrontOrigin=$pgSite; DashboardUrl="$pgSite/"}
```

The update grants the specified OAC/distribution access to the web bucket, permits direct Intake POSTs from the portal origin and registers Cognito callback/logout URLs. Check that both URLs end in `/`. [CloudFront OAC for S3](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html) · [Managed origin request policies](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-managed-origin-request-policies.html)

4. Create the demo users and seed the three SHA-256-pinned fixtures. The script prompts for three email addresses controlled by your group and hides password input. For H02, create `demo-reporter-2` in **Cognito → Users**, using a separate email address, add it to `REPORTER` and sign in once to replace its temporary password. In **SNS → Topics → PhishGuard-SecOps → Create subscription**, select `Email` and enter the SecOps address. Follow **Confirm subscription** in the email and verify `Confirmed` status.

```powershell
.\.venv\Scripts\python.exe scripts/create_demo_users.py --profile $pgProfile
.\.venv\Scripts\python.exe scripts/seed_demo_fixtures.py --profile $pgProfile
```

5. Enter the Cognito prefix to build the live configuration. The `VITE_*` values below are public SPA configuration. Secrets and AWS credentials belong in the deployment environment.

```powershell
$pgPrefix = (Read-Host 'Prefix Cognito da tao').Trim()
$pgEvidence = $pgOut['EvidenceBucketName']
$envFile = @"
VITE_USE_MOCK=false
VITE_COGNITO_REGION=$pgRegion
VITE_COGNITO_USER_POOL_ID=$($pgOut['UserPoolId'])
VITE_COGNITO_CLIENT_ID=$($pgOut['UserPoolClientId'])
VITE_COGNITO_DOMAIN=https://$pgPrefix.auth.$pgRegion.amazoncognito.com
VITE_COGNITO_REDIRECT_URI=$pgSite/
VITE_COGNITO_LOGOUT_URI=$pgSite/
VITE_COGNITO_SCOPES=openid email profile
VITE_EVIDENCE_ALLOWED_HOSTS=$pgEvidence.s3.$pgRegion.amazonaws.com,$pgEvidence.s3.amazonaws.com
"@
[IO.File]::WriteAllText((Join-Path $PWD 'frontend/.env.production.local'), $envFile)
Push-Location frontend
try {
  npm ci
  if ($LASTEXITCODE -ne 0) { throw 'npm ci that bai.' }
  npm run build
  if ($LASTEXITCODE -ne 0) { throw 'Build web that bai.' }
  $web = 's3://' + $pgOut['WebBucketName'] + '/'
  aws s3 sync dist/ $web --exclude index.html --region $pgRegion --profile $pgProfile
  if ($LASTEXITCODE -ne 0) { throw 'Upload assets that bai. Kiem tra ListBucket va PutObject tren web bucket.' }
  aws s3 cp dist/index.html ($web + 'index.html') --content-type text/html --cache-control no-cache --region $pgRegion --profile $pgProfile
  if ($LASTEXITCODE -ne 0) { throw 'Upload index that bai.' }
} finally { Pop-Location }
$inv = aws cloudfront create-invalidation --distribution-id $pgDist --paths "/" "/index.html" --profile $pgProfile --query Invalidation.Id --output text
if ($LASTEXITCODE -ne 0) { throw "CloudFront invalidation failed" }
aws cloudfront wait invalidation-completed --distribution-id $pgDist --id $inv --profile $pgProfile
```

**Check before continuing.** Open the CloudFront URL, sign in and submit the benign notice “Máy in tầng hai đang được bảo trì. Có thể dùng máy in ở tầng một.” In the browser Network panel, `POST /api/reports` should return 202. Upload a separate PNG and check for 201 from `/api/uploads/presign`, followed by 204 from the S3 POST. Use the returned incident ID to open its detail page.

### Check API routing

Open `/api/*` under Behaviors. The screenshot shows `phishguard-api` accepting POST with CachingDisabled and AllViewerExceptHostHeader. Check these three settings when the page loads but report submission fails.

![CloudFront API behavior, methods and request policies.]({{< asset "images/phishguard/cf-api.png" >}})

Verify `VITE_USE_MOCK=false`, the Cognito domain, pool/client, callback and evidence bucket against the stack. Confirm CloudFront serves the hashed JS named by the built `dist/index.html`. [Check the three results observed on AWS](../../verify/demo/#aws-results).

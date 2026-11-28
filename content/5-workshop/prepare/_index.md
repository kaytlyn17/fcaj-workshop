---
title: "5.2 · Prerequisites"
weight: 2
chapter: false
aliases:
  - /5-workshop/02-prerequisites/
---

Use a lab account you are authorized to manage and select **US East (N. Virginia), us-east-1**. Some Lambda, table, user-pool-group and IAM-role names are fixed in the template. If the account already contains PhishGuard, start by inspecting the existing stack. Use a separate lab account for an independent deployment from scratch.

- Install Git, Python 3.12 x64, Node.js 22 LTS, AWS CLI v2 and AWS SAM CLI. Docker is needed only for Linux container builds. The deploying identity needs stack management, S3 packaging and resource deployment permissions, including permission to pass the appropriate service roles. The billing operator needs access to Bills, Cost Explorer and Budgets.
- Prepare DNS access to a receiving domain or subdomain. Use three email addresses controlled by your group for reporter, admin and auditor, plus a second reporter account for user-isolation tests.
- Configure the AWS profile `phishguard` using the account's sign-in mechanism. This profile selects the AWS deployment identity. The `demo-*` accounts sign in to the application through Cognito.
- Extract [PhishGuard-Lab.zip]({{< asset "downloads/PhishGuard-Lab.zip" >}}). The `lab/` directory contains the application, and `tools/` contains the configuration inventory tool. The PowerShell commands below share one terminal session in `lab/`.

```powershell
Set-Location .\lab
$pgProfile = 'phishguard'
$pgRegion = 'us-east-1'
$pgStack = 'phishguard-ai'
aws sts get-caller-identity --profile $pgProfile --query '{Account:Account,Arn:Arn}'
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements-dev.txt
sam --version
node --version
Test-Path .\layers\ocr-vie-py312-x86_64.zip
```

**Check before continuing.** STS should identify the intended lab account, Python should be 3.12, and `Test-Path` should return `True`. If an IAM Identity Center session has expired, run `aws sso login --profile phishguard`.

In the same terminal, check the QR layer and bundled playbooks. The expected results are True and six files. If your AWS profile has a different name, update `$pgProfile` to match it.

```powershell
Test-Path .\layers\qr-py312-x86_64.zip
(Get-ChildItem .\kb-docs\playbooks\*.txt).Count
```

## Test tools included in the lab

Some filenames retain names from earlier development stages. Use the following table to select a tool; workshop commands refer to these exact filenames.

| File in `scripts/` | Purpose |
|---|---|
| `phase2b_smoke.py` | Smoke-test APIs, authentication, authorization, repeated submissions and a benign report after backend deployment. |
| `verify_group1_upload.py` | Check presigned uploads, file limits, OCR and blank images. |
| `verify_group2_samples.py` | Check email fixtures DEMO_2/3 and their processing branches. |
| `verify_admin_api.py` | Check admin/auditor permissions, raw-evidence access and approval decisions. |
| `check_phase_h_local.py` | Run H regressions with local AWS simulation, including H04 and the partial-failure portion of H13. |
| `check_phase_h_live.py` | Check H01–H03 on AWS, or a late H14 decision using `--expired`. |
| `check_playbooks.py` | Query the KB and check for playbooks 04/05/06 in the returned sources. |

A message such as `GATE: PHASE_2B_SMOKE_PASS` is a script result label. It confirms only the checks performed by that script, not the whole system. `SKIPPED` and `UNVERIFIED` retain their meanings of not executed or not verified.

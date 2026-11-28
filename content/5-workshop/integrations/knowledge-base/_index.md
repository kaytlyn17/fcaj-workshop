---
title: "5.4.2 · Playbooks and Bedrock"
weight: 2
chapter: false
---

Images are processed by Tesseract in ExtractRedact. Bedrock receives processed text together with playbook passages retrieved from the Knowledge Base. This section configures the document source, Guardrail and model, then checks retrieval and synthesis separately.

1. Prepare the bundled playbooks. `infra/phishguard-kb-prereqs.yaml` creates the vector bucket, the `playbooks` index with 1024 dimensions and cosine distance, a KB role and a setup policy attached to the specified IAM user. For IAM Identity Center, an administrator should provision the corresponding resources and grant setup permissions to the role instead of attaching the policy to an IAM user.
2. For an IAM-user-based lab, run the following after the main stack creates the KB document bucket. The script checks the index, creates or reuses the KB/data source, waits for ingestion and tests Retrieve.

```powershell
$kbDocs = $pgOut['KbDocsBucketName']
aws s3 sync .\kb-docs\playbooks\ "s3://$kbDocs/playbooks/" --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Upload playbook that bai.' }
$kbUser = Read-Host 'Ten IAM user thuc hien setup KB trong tai khoan lab'
aws cloudformation deploy --template-file infra/phishguard-kb-prereqs.yaml --stack-name phishguard-kb-prereqs --capabilities CAPABILITY_NAMED_IAM --parameter-overrides "SetupUserName=$kbUser" --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Chua tao duoc tai nguyen KB.' }
.\.venv\Scripts\python.exe scripts/setup_phishguard_kb.py --profile $pgProfile
```

| Bedrock / S3 Vectors setting | Lab value |
|---|---|
| Vector store and index | Amazon S3 Vectors · `playbooks` |
| Embedding | Titan Text Embeddings V2, float32, 1024 dimensions, cosine |
| Source and retrieval check | The KB document bucket's `playbooks/` prefix. The latest ingestion should be COMPLETE, and Retrieve should return a document from that prefix. |

3. Record `KnowledgeBaseId` when the script reports `GATE: PHISHGUARD_KB_RETRIEVE_PASS`. In **Bedrock → Guardrails**, create an input **Prompt attacks** filter, configure blocking, publish a version and record its ID/version. In the model catalog, confirm that the account can invoke Nova Lite v1 or an available inference profile for the region. When using a profile, set its `ModelId` and provide the precise profile and destination model ARNs through `ModelInvokeResourceArns`.
4. Update the backend and run `DEMO_2` to inspect synthesis. If the result is `FALLBACK`, read `fallback_reason` and check ingestion status, Guardrail ID/version and Synthesize's Retrieve/InvokeModel permissions.

```powershell
$kb = Read-Host 'KnowledgeBaseId'
$gr = Read-Host 'GuardrailId'
$gv = Read-Host 'GuardrailVersion da phat hanh'
Update-PgStack @{KnowledgeBaseId=$kb; GuardrailId=$gr; GuardrailVersion=$gv}
```

### Verify the six indexed playbooks

In S3, open the bucket returned by `KbDocsBucketName` and its `playbooks/` prefix. The first three documents cover the SecOps handbook, transfer verification and phishing indicators. Document 04 covers forwarded email and DMARC, 05 covers HIGH reports without a supported action target, and 06 covers benefits messages with a deadline.

The historical screenshot below shows five documents at the earlier sync. When running a new sync, record the job ID and failed-document count. Check retrieval separately in **Test knowledge base** with the questions below. The bundled tool checks the top three sources.

```powershell
.\.venv\Scripts\python.exe .\scripts\check_playbooks.py --kb $kb --profile $pgProfile
```

| Test question | Expected source and interpretation |
|---|---|
| An EML's Return-Path differs from From. Is that enough to conclude that the original email failed DMARC? | Look for playbook 04. Distinguish authentication of the outer email from headers supplied in a forwarded message. |
| A HIGH incident has no domain supported well enough to block. What should an analyst do next? | Look for playbook 05. Review the indicators and route the case for manual investigation. |
| A benefits SMS asks the reader to follow a link before a deadline. How should the recipient verify it? | Look for `06-nhan-tro-cap-qua-lien-ket.txt` and compare its advice with the incident rules and proposals. |

Inspect the returned filenames and passages. Then open H05/H11 incidents in the portal and inspect their actual citations or fallback reason. Interactive KB questions and application-generated queries can return different sources.

![Before playbook 06, the ingestion scanned five documents, added two and reported zero failures. Five documents were indexed at the time.]({{< asset "images/phishguard/kb-sync.png" >}})

For an existing stack, open **Knowledge Bases → Data sources → Documents** and match the URI and status. If the document was added in the console and that URI is already `INDEXED`, proceed to Retrieve. If only the S3 object was uploaded and that version is missing from the index, sync the correct data source and wait for `COMPLETE`.

The AWS check found all 6/6 playbooks `INDEXED`. When asked directly how to handle a BHTN benefits SMS with a link and a deadline, Retrieve returned playbooks 04, 06 and 05 in that order. The synthesis for SMS incident `INC-9c67814b7c0a` cited playbooks 03 and 01. [Results for the three AWS incidents](../../verify/demo/#aws-results).

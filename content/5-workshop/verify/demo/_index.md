---
title: "5.5.4 · Trace a demo incident"
weight: 4
chapter: false
---

![The three fixed demo fixtures available in the PhishGuard portal.]({{< asset "images/phishguard/samples.png" >}})

The three fixtures below exercise different branches on the same deployment. Scores depend on extracted content and the blocklist at execution time. Record the blocklist state when comparing two runs.

| Fixture | Procedure | Result to inspect |
|---|---|---|
| **1 · Executive impersonation image** | Sign in as a reporter, submit the image fixture and inspect the OCR text and triggered rules. Switch to an admin, review the target and mode, then approve. | Reference expectation: 65/HIGH, `FINANCE_VERIFICATION_ALERT`, SNS and audit, ending at `SIMULATED`. OCR quality can change the matched rules. |
| **2 · Bank impersonation email** | Inspect the defanged IOC, the lookalike-domain finding and the proposed domain block. | Reference expectation: 80/CRITICAL, `BLOCK_DOMAIN`; DRY_RUN approval records `APPROVED_DRY_RUN` internally. It does not block a firewall or email gateway. |
| **3 · Prompt-injection email** | Compare rules and scores with fixture 2. Inspect the injection flag, synthesis status and fallback reason, and check Synthesize permissions and the role allowed to approve. | The model cannot change scores or execute containment. Record the actual Guardrail outcome. An IOC already marked SEED/APPROVED_CONFIRMED can raise the score to 100. |

### Executive impersonation image

For incident `INC-2d27b9b3353e`, OCR extracted an urgent request to transfer VND 150 million to an unfamiliar account. The matched rules were `FINANCIAL_OR_OTP_REQUEST` (30), `EXEC_IMPERSONATION` (25) and `URGENCY_PRESSURE` (10), totaling 65/100. The detail screenshot shows a fallback summary. This is a different DEMO_1 run from the one in the QR test table.

![Executive impersonation image with three matched rules and a score of 65.]({{< asset "images/phishguard/ceo-ocr-rules.png" >}})

### Approving a domain proposal

In DEMO_2, the approval panel identifies the `BLOCK_DOMAIN` action, target domain and `DRY_RUN` mode. The screenshot shows a pending proposal. After approval, the lab records a simulated outcome and an audit entry. For a new domain, `APPROVED_DRY_RUN` does not activate `KNOWN_BLOCKED_IOC` on subsequent reports.

![The approval panel shows BLOCK_DOMAIN and DRY_RUN.]({{< asset "images/phishguard/domain-proposal-dry-run.png" >}})

In `INTERNAL_LIST` mode, approval of a new domain records `APPROVED_CONFIRMED`, which PhishGuard uses to score later reports. Neither mode updates an employee's firewall, DNS or email gateway. AWS WAF has a separate purpose: filtering requests to the PhishGuard portal through CloudFront.

![Vietnamese text extracted from a benign image report.]({{< asset "images/phishguard/ocr-vi.png" >}})

*The printer-maintenance notice provides a benign control showing Vietnamese diacritics in the OCR output and a low rule score in the earlier UI. The updated portal displays Unknown when evidence is insufficient.*

### Interpreting a run

An incident ID connects the reporter, raw version, processed content, rules, proposal and admin decision. Synthesis explains evidence and reference material; the Policy Engine still determines scores and actions. A Succeeded execution means that the workflow completed normally. Its business outcome may be `NO_ACTION`, `NEEDS_REVIEW` or another state.

To try the SMS case, sign in as a reporter and upload `fixtures/regression/sms.png` from the ZIP. Open the incident to inspect its rules and citations. For the URL case, submit the text `Xem lịch sinh hoạt tại www.calendar.example.org.` separately and open the result using the Unknown filter.

### Limits of the fixtures

The fixtures demonstrate selected processing branches. H01–H15 test MVP behavior, permissions and recovery. Accuracy outside these fixtures, false-positive rates and time saved for SecOps remain unmeasured. Redaction may leave names and addresses; Governance retention can be bypassed with appropriate permission; demo containment records a simulation within the application.

### Results observed on AWS {#aws-results}

| Test case | Rule score | Assessment and workflow | Proposed action | Synthesis |
| --- | ---: | --- | --- | --- |
| BHTN benefits SMS image from the ZIP · `INC-9c67814b7c0a` · OCR 91.17% | 50/100 | `SUSPICIOUS` → `NEEDS_REVIEW` | No domain block proposed. | `OK`, cited playbooks 03 and 01. |
| Unverified URL · `INC-138575acf8f2` | 0/100 | `UNKNOWN / UNVERIFIED_URL` → `NO_ACTION` | No action proposed. | `FALLBACK` due to `INVALID_MODEL_OUTPUT`. |
| DEMO_1 · `INC-74bc1b87888b` | 65/100 | `SUSPICIOUS` → `PENDING_APPROVAL` | `FINANCE_VERIFICATION_ALERT` in `DRY_RUN` mode. The run stopped before approval. | `FALLBACK` due to `GUARDRAIL_PROMPT_ATTACK`. |

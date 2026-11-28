---
title: "5.4.1 · Receive email with SES"
weight: 1
chapter: false
---

Select **us-east-1** in SES. Create a domain identity that you control, publish the verification/DKIM records supplied by the console and wait for `Verified`. Add an MX record with priority 10 pointing to `inbound-smtp.us-east-1.amazonaws.com` for the receiving domain. If the main domain already receives mail through another system, use a lab subdomain and its `soc@` address, preserving the main domain's MX records.

1. In **Email receiving → Receipt rule sets**, open the active rule set or create one for the lab. If the account already processes business mail, add a rule to the existing set and review the rules that precede it.
2. Create a rule for the exact receiving address. The first action is **Deliver to S3**, using the stack's Intake bucket and `inbound/` prefix. The next is **Invoke Lambda**, selecting `PhishGuard-Ingest` with invocation type `Event` (asynchronous). Enable the rule. The stack provides the relevant SES invocation and prefix-write permissions.
3. Forward a benign email from a verified Cognito mailbox in `AllowedSenderDomains`. Find the new object under `inbound/` and the corresponding dashboard incident. Expected fields are `source_channel=SES_EMAIL` and `reporter_identity_status=VERIFIED_SES_DMARC`. If logs show `UNVERIFIED_SENDER`, inspect the outer message address, allowed domains, `email_verified` and SES verdict.

[SES MX configuration](https://docs.aws.amazon.com/ses/latest/dg/receiving-email-mx-record.html) · [Receipt rules](https://docs.aws.amazon.com/ses/latest/dg/receiving-email-receipt-rules-console-walkthrough.html)

### Check the receiving domain

The screenshot shows DKIM configuration as Successful and signatures Enabled. Next, check the rule in the active rule set and match a forwarded email to an incident.

![DKIM configuration for the receiving domain.]({{< asset "images/phishguard/mail-dkim.png" >}})

### Invoke Ingest after storing the email

Select PhishGuard-Ingest and Event invocation for the receipt rule's second action. The first action must store the MIME message under `inbound/`. The screenshot records this configuration choice.

![The receipt rule's second action invokes Ingest asynchronously.]({{< asset "images/phishguard/ses-invoke.png" >}})

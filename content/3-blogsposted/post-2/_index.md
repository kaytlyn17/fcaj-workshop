---
title: "AWS KMS and envelope encryption: Who can read an encrypted S3 object?"
weight: 2
chapter: false
menuTitle: "3.2. Blog 2"
---

Why can a user download a readable PDF even though the S3 object is encrypted with AWS KMS? I followed a single upload and download to see where encryption ends and access control takes over.

**On upload**, in the basic SSE-KMS flow, S3 asks KMS for a data key. S3 encrypts the file with the plaintext data key, discards its plaintext copy and stores the encrypted copy of the key with the object. **On download**, an authorised read allows S3 to recover the data key and return the file contents. For that read I would review both the caller's `s3:GetObject` permission and the effective permission to use `kms:Decrypt` on the key.

### What changes when I enable S3 Bucket Keys?

Consider a policy that checks `kms:EncryptionContext:aws:s3:arn` for a report in `example-documents`. The value S3 supplies by default depends on how the object was encrypted.

| SSE-KMS configuration for the object | Default ARN in the encryption context |
| --- | --- |
| Without an S3 Bucket Key | `arn:aws:s3:::example-documents/reports/quarterly.pdf` |
| With an S3 Bucket Key | `arn:aws:s3:::example-documents` |

S3 Bucket Keys reduce calls to KMS. A condition that expects the full object ARN can consequently fail to match. Before enabling Bucket Keys, I would review policies that use the encryption context and check where the team records individual reads. With fewer KMS events and a bucket-level ARN, S3 object-level data events matter for that investigation.

## AWS documentation

- [Amazon S3 SSE-KMS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)
- [S3 Bucket Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html)
- [AWS KMS encryption context](https://docs.aws.amazon.com/kms/latest/developerguide/encrypt_context.html)

![Blog Picture]({{< asset "images/blogposted_2.png" >}})

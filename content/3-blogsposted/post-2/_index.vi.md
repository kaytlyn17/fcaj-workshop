---
title: "AWS KMS và envelope encryption: Ai đọc được một object S3 đã mã hóa?"
weight: 2
chapter: false
menuTitle: "3.2. Blog 2"
---

Vì sao một file PDF đã mã hóa bằng AWS KMS trên S3 vẫn mở được ngay sau khi tải xuống? Mình lần theo một lượt tải lên và một lượt tải xuống để hiểu vai trò của từng khóa và quyền nào cho phép người dùng đọc file.

**Khi tải lên**, trong luồng SSE-KMS cơ bản, S3 yêu cầu KMS tạo khóa dữ liệu. S3 dùng bản rõ của khóa để mã hóa file, loại bỏ bản rõ sau khi dùng và lưu bản khóa đã mã hóa cùng object. **Khi tải xuống**, nếu yêu cầu được phép, S3 lấy lại khóa dữ liệu để giải mã và trả nội dung cho người đọc. Vì vậy, khi rà soát quyền đọc file, mình sẽ kiểm tra cả `s3:GetObject` lẫn quyền `kms:Decrypt` có hiệu lực trên khóa KMS.

### Bật S3 Bucket Keys thì điều gì thay đổi?

Giả sử policy dùng điều kiện `kms:EncryptionContext:aws:s3:arn` cho một báo cáo trong bucket `example-documents`. Giá trị mặc định mà S3 gửi phụ thuộc vào cách object được mã hóa.

| Cấu hình SSE-KMS của object | ARN mặc định trong encryption context |
| --- | --- |
| Không dùng S3 Bucket Key | `arn:aws:s3:::example-documents/reports/quarterly.pdf` |
| Có dùng S3 Bucket Key | `arn:aws:s3:::example-documents` |

Bucket Keys giúp S3 giảm số lần gọi KMS. Đổi lại, điều kiện policy đang đòi ARN đầy đủ của từng object có thể hết khớp. Trước khi bật, mình sẽ rà các policy dùng encryption context và kiểm tra nơi lưu lịch sử đọc từng file. Khi sự kiện KMS thưa hơn và chỉ mang ARN của bucket, S3 data events ở cấp object giúp trả lời câu hỏi đó.

## Tài liệu AWS

- [Amazon S3 SSE-KMS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)
- [S3 Bucket Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html)
- [AWS KMS encryption context](https://docs.aws.amazon.com/kms/latest/developerguide/encrypt_context.html)

![Blog Picture]({{< asset "images/blogposted_2.png" >}})

---
title: "S3 đã bật Block Public Access thì dữ liệu đã an toàn chưa?"
weight: 1
chapter: false
menuTitle: "3.1. Blog 1"
---

Một nhóm lưu báo cáo trong bucket S3 riêng tư. Ứng dụng nội bộ được phép đọc các báo cáo đó. Nếu thông tin xác thực của ứng dụng bị lộ, nhóm có thể dựa vào đâu để biết báo cáo nào đã bị đọc? Mình dùng tình huống này để tìm hiểu ba việc cần làm khi bảo vệ dữ liệu trong S3.

1. **Giới hạn quyền đọc.** S3 Block Public Access kiểm soát việc cấp quyền công khai qua policy và ACL. Một role có quyền `s3:GetObject` vẫn đọc được các object trong phạm vi cho phép. Khi rà soát, mình sẽ bắt đầu từ các prefix mà từng ứng dụng thực sự cần đọc.
2. **Ghi lại thao tác trên object.** Để tra ai đã gọi `GetObject`, lúc nào và với object nào, mình sẽ cấu hình CloudTrail ghi S3 data events cho thao tác đọc ở bucket báo cáo. Loại sự kiện này cần được chọn rõ trong cấu hình trail.
3. **Đưa cảnh báo tới người phụ trách.** GuardDuty S3 Protection có thể phát hiện hoạt động truy cập bất thường. Một rule của EventBridge có thể chuyển các finding phù hợp đến SNS để gửi email cho người đã xác nhận đăng ký. GuardDuty vẫn có thể phân tích hoạt động S3 độc lập với trail mình cấu hình để lưu nhật ký điều tra.

Để thử đường gửi cảnh báo, mình sẽ dùng một sample finding của GuardDuty khớp với rule rồi kiểm tra hộp thư. Với cảnh báo thật, bước tiếp theo là đối chiếu thời điểm và đối tượng liên quan với lịch sử truy cập của bucket.

## Tài liệu AWS

- [S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html)
- [CloudTrail data events](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html)
- [GuardDuty S3 Protection](https://docs.aws.amazon.com/guardduty/latest/ug/s3-protection.html)

![Blog Picture]({{< asset "images/blogposted_1.png" >}})


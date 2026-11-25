---
title: "1.2. Worklog Tuần 2"

weight: 2
chapter: false
---

21/09/2026 - 25/09/2026

## Mục tiêu

Triển khai S3 và RDS. Thử Lightsail cho máy ảo và container. Quan sát khả năng co giãn, tự phục hồi của Auto Scaling.

## Công việc theo ngày

| Ngày | Công việc | Bắt đầu | Hoàn thành | Tài liệu lab |
|---|---|---|---|---|
| Thứ hai | Triển khai web tĩnh trên S3, cấu hình policy, versioning và replication. Dùng CloudFront để giới hạn truy cập trực tiếp. | 21/09/2026 | 21/09/2026 | [000057](https://000057.awsstudygroup.com/) |
| Thứ ba | Tạo DB subnet group, Security Group và RDS. Kết nối ứng dụng trên EC2, chụp/phục hồi snapshot, phân biệt Multi-AZ và read replica. | 22/09/2026 | 22/09/2026 | [000005](https://000005.awsstudygroup.com/) |
| Thứ tư | Dựng WordPress và cơ sở dữ liệu trên Lightsail, thử snapshot, nâng cấu hình và alarm. Triển khai container bằng image public và image tự build. | 23/09/2026 | 23/09/2026 | [000045](https://000045.awsstudygroup.com/) · [000046](https://000046.awsstudygroup.com/) |
| Thứ năm | Tạo launch template, target group, ALB, Auto Scaling Group và target tracking. Tạo tải để quan sát tăng và giảm số máy. | 24/09/2026 | 24/09/2026 | [000006](https://000006.awsstudygroup.com/) |
| Thứ sáu | So sánh Lightsail/EC2, S3/EBS/EFS và Multi-AZ/read replica. Vẽ sơ đồ, dọn lab và xem Billing. | 25/09/2026 | 25/09/2026 | [cloudjourney](https://cloudjourney.awsstudygroup.com/) |

## Kết quả trong tuần

Phân phối web S3 qua CloudFront, kết nối RDS từ EC2 và phục hồi snapshot. So sánh Lightsail với EC2, quan sát ALB và Auto Scaling khi tải thay đổi, vẽ kiến trúc bằng draw.io.

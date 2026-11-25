---
title: "1.6. Worklog Tuần 6"

weight: 6
chapter: false
---

19/10/2026 - 23/10/2026

## Mục tiêu

Tự động hóa thao tác vận hành, truy vấn log, dùng tag kiểm soát tài nguyên và quản trị máy chủ bằng Systems Manager.

## Công việc theo ngày

| Ngày | Công việc | Bắt đầu | Hoàn thành | Tài liệu lab |
|---|---|---|---|---|
| Thứ hai | Gắn tag cho EC2, tạo role và hai Lambda start/stop theo lịch EventBridge, gửi thông báo Slack. | 19/10/2026 | 19/10/2026 | [000022](https://000022.awsstudygroup.com/) |
| Thứ ba | Cài Grafana trên EC2. Thử biểu thức metric, dynamic label, Container Insights và sử dụng số đo để rút ngắn thời gian xử lý sự cố. | 20/10/2026 | 20/10/2026 | [000029](https://000029.awsstudygroup.com/) · [000036](https://000036.awsstudygroup.com/) |
| Thứ tư | Tạo Resource Groups theo tag, viết điều kiện IAM và kiểm quyền trên EC2 có/không có tag khi đổi role. | 21/10/2026 | 21/10/2026 | [000027](https://000027.awsstudygroup.com/) · [000028](https://000028.awsstudygroup.com/) |
| Thứ năm | Gắn role Systems Manager, dùng Patch Manager và Run Command trên nhiều máy. | 22/10/2026 | 22/10/2026 | [000031](https://000031.awsstudygroup.com/) |
| Thứ sáu | Dùng Session Manager với các endpoint ssm, ssmmessages, ec2messages. Ghi log vào S3, so sánh ba công cụ vận hành và dọn lab. | 23/10/2026 | 23/10/2026 | [000058](https://000058.awsstudygroup.com/) |

## Kết quả trong tuần

Lập lịch khởi động/dừng EC2. Dùng Grafana, Logs Insights và điều kiện IAM theo tag. Quản trị qua Session Manager và phân biệt các chức năng của Systems Manager.

---
title: "5.6 · Đánh giá Well-Architected"
weight: 6
chapter: false
---

Phần này xem xét cách PhishGuard được vận hành, các lựa chọn thiết kế và những giới hạn còn lại. Khi xử lý một sự cố, ba nơi thường cần mở cùng nhau là trang chi tiết trên portal, execution của Step Functions và phiên bản bằng chứng trong S3. Mã incident nối các dữ liệu này với nhau.

## Đối chiếu kiến trúc với cấu hình

CloudFront phục vụ portal từ S3 và chuyển `/api/*` tới HTTP API. WAF gắn với distribution. Hai đường nhận báo cáo qua web và SES hội tụ tại Ingest; bản gốc được lưu trước khi workflow bắt đầu. Lambda dùng mạng do AWS quản lý, template không khai báo subnet ứng dụng.

Trong workflow, `PrepareAndWaitApproval` là Task dùng `lambda:invoke.waitForTaskToken`. PrepareApproval lưu token và gửi thông báo rồi kết thúc lần gọi. Task tiếp tục chờ callback tối đa 48 giờ theo cấu hình mặc định. Hai khối Prepare approval và Wait for approval trên sơ đồ mô tả hai phần của cùng cơ chế này.

## Các quyết định theo AWS Well-Architected

| Trụ cột | Cách áp dụng trong PhishGuard | Kết quả và giới hạn |
|---|---|---|
| Operational Excellence | Backend triển khai bằng SAM; change set giúp kiểm tra tác động trước khi cập nhật. Hai alarm theo dõi workflow thất bại và DLQ có bản tin, gửi thông báo qua SNS. | Ảnh ở [5.5.1](../verify/account/) cho thấy alarm OK và action đã bật tại thời điểm chụp. Chưa có minh chứng một lần alarm chuyển trạng thái và email cảnh báo tương ứng. |
| Security | JWT và nhóm Cognito kiểm soát API. Raw evidence tách riêng, có SSE-KMS, versioning và Governance 7 ngày. Admin API kiểm quyền trước khi cấp URL raw hoặc gửi quyết định. | Kiểm thử phân quyền được mô tả ở H02/H15. Governance có thể bị bypass bởi danh tính đủ quyền. Che PII theo mẫu có thể bỏ sót tên, địa chỉ và nội dung OCR sai. |
| Reliability | Ghi có điều kiện và execution theo incident hỗ trợ xử lý retry. Ingest bất đồng bộ có DLQ. Workflow có timeout; bước tổng hợp có fallback. | H04 và phần lỗi giữa chừng của H13 được kiểm tra bằng mô phỏng cục bộ. H14 có ảnh quá hạn và phản hồi 409. RTO/RPO cho sự cố toàn Region chưa được đo. |
| Performance Efficiency | Upload trực tiếp tới S3 giảm dữ liệu qua API. ExtractRedact dùng x86_64, 512 MiB và layer OCR/QR. Callback không giữ Lambda chạy suốt thời gian chờ người duyệt. | Chưa có benchmark p50/p95. Khi đo, cần tách ảnh khỏi văn bản và xem Duration, Billed Duration, Max Memory Used cùng Throttles. |
| Cost Optimization | Dự toán dựa trên số báo cáo, lượng ảnh, token và các khoản phí cố định. Intake có lifecycle, log có retention. | [5.7](../cost/) tách ngân sách, Calculator và số liệu Cost Explorer. WAF chiếm phần lớn dự toán ở tải thấp; số tiền thực tế riêng cho project chưa được đối chiếu đầy đủ. |
| Sustainability | Xử lý theo sự kiện, không duy trì máy chủ ứng dụng chạy chờ. Dữ liệu tạm có thời hạn lưu. | Cần kiểm tra cả phiên bản object cũ khi thu dọn. Dự án chưa đo mức sử dụng năng lượng hoặc phát thải. |

## Xử lý khi DLQ có bản tin

1. Xác định thời điểm alarm và tìm lỗi của Ingest trong CloudWatch Logs.
2. Kiểm tra object đầu vào còn tồn tại, quyền đọc/ghi và phiên bản bằng chứng đã lưu.
3. Tra incident trong DynamoDB và execution cùng tên để biết workflow đã được khởi tạo hay chưa.
4. Sửa nguyên nhân rồi thử lại trên dữ liệu lab. Đối chiếu số incident, phiên bản raw và execution để phát hiện việc xử lý trùng.

Lifecycle của Intake là 3 ngày, vì vậy một bản tin để lâu trong DLQ có thể trỏ tới dữ liệu đầu vào đã hết hạn. Cần xác định còn dữ liệu nào có thể khôi phục trước khi gửi lại sự kiện.

## Phạm vi bảo vệ của bản lab

WAF lọc request qua CloudFront. Upload trực tiếp tới S3 được kiểm soát bằng presigned policy và Ingest; email đi qua SES. Template vẫn để HTTP API có endpoint `execute-api` và chưa ép mọi lời gọi API đi qua CloudFront. JWT, phân quyền và throttle của API vẫn áp dụng cho đường gọi trực tiếp đó.

Cognito trong template đặt MFA ở trạng thái OFF. Audit raw evidence ghi việc cấp URL 60 giây, chưa chứng minh người dùng đã tải object. Các mục này cần được đánh giá thêm trước khi dùng dữ liệu thật hoặc triển khai cho tổ chức. Containment hiện chỉ mô phỏng hoặc cập nhật danh sách nội bộ, chưa tác động lên firewall/email gateway.

[AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/the-pillars-of-the-framework.html)

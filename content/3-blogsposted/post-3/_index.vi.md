---
title: "SQS FIFO có thực sự đảm bảo “exactly-once processing”?"
weight: 3
chapter: false
menuTitle: "3.3. Blog 3"
---

Giả sử Lambda nhận sự kiện `evt-987` của đơn hàng `1042`, ghi thành công vào DynamoDB rồi bị timeout trước khi lần chạy kết thúc. Sau thời gian ẩn tin nhắn (*visibility timeout*), chính tin nhắn đó có thể xuất hiện lại. Lần xử lý thứ hai sẽ làm gì với đơn hàng?

**Ở hàng đợi**, SQS FIFO giữ thứ tự trong từng `MessageGroupId`. Nếu bên gửi thử lại với cùng `MessageDeduplicationId` trong vòng năm phút, SQS loại lần gửi trùng. Trong ví dụ trên, bên gửi chỉ gửi một lần. Chính tin nhắn đang có trong hàng đợi được giao lại cho Lambda xử lý.

**Ở cơ sở dữ liệu**, nếu yêu cầu chỉ là tạo một bản ghi cho mỗi sự kiện, mình có thể dùng `eventId` làm khóa phân vùng và gọi `PutItem` với điều kiện `attribute_not_exists(eventId)`. Lần đầu, DynamoDB tạo bản ghi. Khi nhận lại cùng sự kiện, điều kiện thất bại và bản ghi cũ được giữ nguyên. Nếu quy trình còn thu tiền hoặc gửi email, từng thao tác đó cũng cần cách nhận diện lần thử lại.

**Khi xử lý theo lô**, nếu đã bật phản hồi lỗi từng phần, AWS khuyến nghị dùng FIFO thì dừng ở lỗi đầu tiên và trả về cả tin nhắn lỗi lẫn các tin chưa xử lý. Có thể chuyển tin lỗi nhiều lần sang hàng đợi lỗi FIFO (DLQ) để điều tra. Khi tách một tin ra như vậy, thứ tự hoàn tất nghiệp vụ có thể thay đổi.

Câu hỏi mình sẽ dùng để rà thiết kế là: nếu hàm dừng ngay sau lần ghi thành công đầu tiên, lần chạy tiếp theo sẽ làm gì với đơn hàng `1042`?

## Tài liệu AWS

- [FIFO deduplication](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/FIFO-queues-exactly-once-processing.html)
- [Lambda with SQS](https://docs.aws.amazon.com/lambda/latest/dg/with-sqs.html)
- [Lambda batch error handling](https://docs.aws.amazon.com/lambda/latest/dg/services-sqs-errorhandling.html)
- [DynamoDB condition expressions](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Expressions.ConditionExpressions.html)

![Blog Picture]({{< asset "images/blogposted_3.png" >}})

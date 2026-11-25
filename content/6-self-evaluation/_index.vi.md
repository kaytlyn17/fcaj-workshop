---
title: "6. Tự đánh giá"
weight: 6
chapter: false
---

Những tuần đầu, khi học IAM, VPC và EC2, em thường xem việc kết nối được đến máy chủ là đã làm xong bài. Đến các buổi thực hành CloudWatch, tự động hóa và bảo mật, em bắt đầu đặt thêm ba câu hỏi: ai được phép thực hiện thao tác đó, xem kết quả ở đâu và người khác có thể làm lại bằng cách nào? Sự thay đổi trong cách học ấy là điều em thấy rõ nhất khi đọc lại nhật ký FCAJ của mình.

## Bảng tự đánh giá

Em tự xếp mức dựa trên các bài lab, bài viết, sự kiện và đồ án đã có minh chứng đến ngày 24/11/2026. Với những kỹ năng em mới thể hiện trong phạm vi bài tập cá nhân, em chọn mức Khá để phản ánh đúng cơ hội thực hành.

| STT | Tiêu chí | Căn cứ tự đánh giá | Tốt | Khá | Trung bình |
|---:|---|---|:---:|:---:|:---:|
| 1 | Kiến thức chuyên môn | Em giải thích được quan hệ giữa danh tính, mạng, máy chủ và lưu trữ khi triển khai một ứng dụng. Em còn cần thực hành thêm cách chọn dịch vụ khi tải và ngân sách cùng thay đổi. | | ✓ | |
| 2 | Khả năng học hỏi | Từ thao tác trên console, em chuyển sang dùng CLI, đọc template và so kết quả triển khai với cấu hình đã viết. | ✓ | | |
| 3 | Tính chủ động | Khi kết quả lab khác hướng dẫn, em tra lại tài liệu và thử một thay đổi nhỏ trước khi chuyển sang bước kế tiếp. | ✓ | | |
| 4 | Tinh thần trách nhiệm | Em giữ lại bằng chứng cho các kết quả đưa vào báo cáo và ghi riêng những mục tài khoản chưa đủ quyền để xác minh. | ✓ | | |
| 5 | Kỷ luật và quản lý thời gian | Worklog giúp em theo dõi tiến độ theo tuần. Một số phần chú thích ảnh và đối chiếu chi phí vẫn dồn về cuối. | | ✓ | |
| 6 | Tinh thần cầu tiến | Em dùng góp ý về sơ đồ và menu workshop để xem lại đường đi của dữ liệu và viết lại hướng dẫn theo góc nhìn của người học. | ✓ | | |
| 7 | Khả năng giao tiếp | Viết nội dung kỹ thuật bằng tiếng Việt và tiếng Anh giúp em chọn thuật ngữ chính xác hơn. Khi trao đổi miệng, em còn có xu hướng giải thích quá nhiều chi tiết. | | ✓ | |
| 8 | Khả năng phối hợp | Em biết chuẩn bị các bước tái hiện để người khác có thể góp ý. Phần lớn minh chứng hiện tại là từ lab cá nhân, nên em muốn có thêm dịp triển khai theo cặp. | | ✓ | |
| 9 | Tác phong làm việc | Em chú ý phạm vi tài khoản, quyền truy cập và dữ liệu xuất hiện trong ảnh trước khi dùng ảnh làm minh chứng. | ✓ | | |
| 10 | Xử lý vấn đề | Khi stack rollback, em lần từ tài nguyên lỗi về thao tác IAM bị từ chối. Cách này giúp em phân biệt lỗi gốc với các cập nhật bị hủy theo. | ✓ | | |
| 11 | Đóng góp vào dự án | Em kết nối kết quả từ các lab bảo mật, giám sát và IaC thành một workshop có hướng dẫn dựng, kiểm tra và thu dọn. | ✓ | | |
| 12 | Đánh giá chung | Em đã có thể tự dựng và giải thích một hệ thống lab. Kinh nghiệm làm việc nhóm, đo hiệu năng và vận hành dài hạn vẫn cần tích lũy. | | ✓ | |

## Điểm mạnh

Em học tốt nhất khi có một kết quả cụ thể để kiểm tra. Khi học giám sát, một biểu đồ có một điểm dữ liệu ban đầu rất khó hiểu. Sau khi đối chiếu thời gian gửi yêu cầu, tên luật và đơn vị đếm, em hiểu vì sao chỉ một điểm cũng có thể trả lời đúng câu hỏi đang kiểm tra. Cách làm này cũng giúp em viết blog: trước khi giải thích cơ chế của dịch vụ, em phải chọn được ví dụ mà người đọc có thể tự đối chiếu.

Một ví dụ khác là tuần học Systems Manager và tuần học IaC. Ở tuần đầu, em quan tâm cách thao tác nhất quán trên tài nguyên. Sang tuần sau, khi đọc một change set trước khi triển khai, em học được cách đặt câu hỏi sớm hơn: thay đổi nào sẽ cập nhật tại chỗ, thay đổi nào có thể thay thế tài nguyên? Hai bài học này liên quan trực tiếp đến cách em lập kế hoạch và bàn giao công việc.

## Những điểm cần cải thiện

Em có thể dành khá nhiều thời gian để làm cho một mục trong lab chạy được, rồi lại thiếu ghi chú về cấu hình ban đầu. Điều đó gây khó khi cần giải thích kết quả vài ngày sau. Các buổi chia sẻ về AI và kiến trúc cũng cho em thấy khoảng cách giữa việc hiểu một bài trình diễn và việc tự chọn giải pháp khi có nhiều phương án hợp lý. Em cần trình bày được cả lý do chọn, chi phí và điều kiện khiến mình đổi lựa chọn.

## Kế hoạch cải thiện

- Ở mỗi lab tiếp theo, em sẽ ghi mục tiêu, cấu hình ban đầu và dấu hiệu thành công trước khi bắt đầu. Sau lab, em sẽ thêm một tình huống lỗi cùng cách tìm nguyên nhân.
- Mỗi tuần em sẽ chọn một quyết định thiết kế, viết phần giải thích ngắn bằng tiếng Anh rồi trình bày miệng trong ba phút cho một bạn học. Em muốn biết đoạn nào người nghe thực sự theo kịp.
- Với bài tập nhóm kế tiếp, em muốn đổi vai giữa người viết hướng dẫn và người chạy thử trên môi trường mới. Phản hồi từ người chạy thử sẽ là cơ sở để chỉnh tài liệu.

Nhìn lại kỳ thực tập đến thời điểm này, em tự xếp mức **Khá**. Điều em muốn giữ từ FCAJ là thói quen chứng minh điều mình vừa học bằng một kết quả mà người khác kiểm tra được.

[Nhật ký theo tuần](../1-worklog/) · [Bài viết kỹ thuật](../3-blogsposted/) · [Sự kiện đã tham dự](../4-eventparticipated/)

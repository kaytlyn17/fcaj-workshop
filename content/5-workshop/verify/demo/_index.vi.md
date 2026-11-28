---
title: "5.5.4 · Theo dấu một sự cố demo"
weight: 4
chapter: false
---

![Ba fixture demo có sẵn trên cổng báo cáo của PhishGuard]({{< asset "images/phishguard/samples.png" >}})

Ba mẫu cố định dưới đây dùng để kiểm tra các nhánh xử lý trên cùng một bản triển khai. Điểm phụ thuộc nội dung trích được và blocklist tại thời điểm chạy; ghi lại trạng thái blocklist khi cần so sánh hai lần thử.

| Mẫu | Cách thử | Kết quả cần kiểm tra |
|---|---|---|
| **1 · Ảnh giả danh lãnh đạo** | Đăng nhập reporter, chạy mẫu ảnh, mở nội dung OCR và từng luật. Chuyển sang admin, đọc mục tiêu và chế độ rồi duyệt. | Mẫu chuẩn kỳ vọng 65/HIGH, `FINANCE_VERIFICATION_ALERT`, SNS và audit, kết thúc `SIMULATED`. Chất lượng OCR có thể làm thay đổi luật. |
| **2 · Email giả mạo ngân hàng** | Mở IOC đã đổi sang dạng văn bản an toàn, chỉ ra vì sao miền giống thương hiệu, rồi mở đề xuất chặn miền. | Mẫu chuẩn 80/CRITICAL, `BLOCK_DOMAIN`, sau duyệt DRY_RUN có bản ghi nội bộ `APPROVED_DRY_RUN`. Chưa có chặn ở firewall/email gateway. |
| **3 · Email chứa prompt injection** | So các luật và điểm với mẫu 2, kiểm cờ injection, trạng thái tổng hợp và lý do fallback nếu có. Đối chiếu quyền của Synthesize và vai trò được phép duyệt. | Model không tự thay điểm hoặc thực thi containment. Ghi đúng kết quả Guardrail của lần chạy. IOC có nguồn SEED/APPROVED_CONFIRMED từ lần trước có thể làm điểm lên 100. |

### Ảnh giả danh lãnh đạo

Ở lượt `INC-2d27b9b3353e`, OCR đọc được yêu cầu chuyển gấp 150 triệu vào tài khoản lạ. Ba luật khớp là `FINANCIAL_OR_OTP_REQUEST` (30), `EXEC_IMPERSONATION` (25) và `URGENCY_PRESSURE` (10), tổng cộng 65/100. Ảnh chi tiết cho thấy bản tổng hợp dự phòng. Đây là một lượt chạy DEMO_1 khác với lượt trong bảng kiểm thử QR.

![Ảnh giả danh giám đốc với ba luật khớp và điểm 65.]({{< asset "images/phishguard/ceo-ocr-rules.png" >}})

### Khi admin duyệt một miền

Trong DEMO_2, màn hình duyệt ghi rõ hành động `BLOCK_DOMAIN`, miền đích và chế độ `DRY_RUN`. Đây là đề xuất đang chờ quyết định. Sau khi admin duyệt, bộ lab ghi kết quả mô phỏng và audit. Với một miền mới, bản ghi `APPROVED_DRY_RUN` không kích hoạt luật `KNOWN_BLOCKED_IOC` ở lần báo cáo tiếp theo.

![Màn hình phê duyệt hiển thị BLOCK_DOMAIN và DRY_RUN.]({{< asset "images/phishguard/domain-proposal-dry-run.png" >}})

Ở chế độ `INTERNAL_LIST`, một miền mới được duyệt mang nguồn `APPROVED_CONFIRMED`. PhishGuard dùng miền đó để chấm báo cáo sau. Cả hai chế độ đều không cập nhật firewall, DNS hay email gateway của nhân viên. AWS WAF có nhiệm vụ riêng là lọc request gửi tới cổng PhishGuard qua CloudFront.

![Phần hiển thị chữ tiếng Việt sau OCR của một báo cáo ảnh vô hại]({{< asset "images/phishguard/ocr-vi.png" >}})

*Thông báo bảo trì máy in cho thấy hệ thống đọc được dấu tiếng Việt và điểm luật thấp trên giao diện cũ. Bản mới hiển thị Chưa xác định khi thiếu căn cứ.*

### Đọc kết quả của một lượt chạy

Từ một mã incident, có thể đối chiếu người gửi, phiên bản raw, nội dung đã xử lý, luật, đề xuất và quyết định của admin. Phần tổng hợp hỗ trợ giải thích bằng chứng và tài liệu tham chiếu; điểm và hành động vẫn do Policy Engine quyết định. Một execution Succeeded chỉ cho biết workflow kết thúc bình thường. Trạng thái nghiệp vụ có thể là `NO_ACTION`, `NEEDS_REVIEW` hoặc một kết quả khác.

Để thử ca SMS, đăng nhập bằng tài khoản reporter, chọn **Tải lên bằng chứng** và gửi ảnh `fixtures/regression/sms.png` trong ZIP. Chọn **Mở sự cố** để đối chiếu luật và nguồn trích dẫn. Với ca URL, gửi riêng văn bản `Xem lịch sinh hoạt tại www.calendar.example.org.` và mở kết quả qua bộ lọc **Chưa xác định**.

### Giới hạn của bộ mẫu

 Fixture cho thấy hệ thống đi đúng một số nhánh xác định. H01–H15 đo hành vi, phân quyền và khả năng phục hồi của MVP. Độ chính xác trên thư ngoài tập mẫu, tỷ lệ cảnh báo nhầm và thời gian tiết kiệm cho SecOps vẫn cần đo. Tên và địa chỉ trong nội dung đã che PII có thể còn sót; S3 Governance có quyền bypass; containment của demo ghi mô phỏng trong phạm vi hệ thống.

### Kết quả quan sát trên AWS {#aws-results}

| Ca kiểm thử | Điểm luật | Kết luận và trạng thái | Đề xuất | Synthesize |
| --- | ---: | --- | --- | --- |
| SMS BHTN từ ảnh trong ZIP · `INC-9c67814b7c0a` · OCR 91,17% | 50/100 | `SUSPICIOUS` → `NEEDS_REVIEW` | Không có đề xuất chặn miền. | `OK`, trích playbook 03 và 01. |
| URL chưa xác minh · `INC-138575acf8f2` | 0/100 | `UNKNOWN / UNVERIFIED_URL` → `NO_ACTION` | Không có đề xuất. | `FALLBACK` do `INVALID_MODEL_OUTPUT`. |
| DEMO_1 · `INC-74bc1b87888b` | 65/100 | `SUSPICIOUS` → `PENDING_APPROVAL` | `FINANCE_VERIFICATION_ALERT` ở chế độ `DRY_RUN`. Lượt này dừng trước khi duyệt. | `FALLBACK` do `GUARDRAIL_PROMPT_ATTACK`. |

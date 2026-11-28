---
title: "5.4.2 · Playbook và Bedrock"
weight: 2
chapter: false
---

Ảnh được xử lý bởi Tesseract trong ExtractRedact. Bedrock nhận nội dung đã qua xử lý cùng các đoạn playbook do Knowledge Base truy xuất. Phần này cấu hình nguồn tài liệu, Guardrail và model, sau đó kiểm tra riêng kết quả truy xuất và kết quả tổng hợp.

1. Đồng bộ các playbook trong gói. File `infra/phishguard-kb-prereqs.yaml` tạo vector bucket, index `playbooks` 1.024 chiều cosine, role cho KB và policy setup gắn với IAM user được chỉ định. Với môi trường IAM Identity Center, quản trị viên triển khai các tài nguyên tương ứng và gán quyền setup cho role được dùng, thay phần đính kèm vào IAM user.
2. Với người triển khai dùng IAM user của lab, chạy các lệnh sau sau khi stack chính đã tạo bucket KB. Script kiểm index, tạo hoặc tái sử dụng KB/data source, đợi đồng bộ rồi thử Retrieve.

```powershell
$kbDocs = $pgOut['KbDocsBucketName']
aws s3 sync .\kb-docs\playbooks\ "s3://$kbDocs/playbooks/" --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Upload playbook that bai.' }
$kbUser = Read-Host 'Ten IAM user thuc hien setup KB trong tai khoan lab'
aws cloudformation deploy --template-file infra/phishguard-kb-prereqs.yaml --stack-name phishguard-kb-prereqs --capabilities CAPABILITY_NAMED_IAM --parameter-overrides "SetupUserName=$kbUser" --region $pgRegion --profile $pgProfile
if ($LASTEXITCODE -ne 0) { throw 'Chua tao duoc tai nguyen KB.' }
.\.venv\Scripts\python.exe scripts/setup_phishguard_kb.py --profile $pgProfile
```

| Trường cần đối chiếu ở Bedrock và S3 Vectors | Giá trị của lab |
|---|---|
| Vector store và index | Amazon S3 Vectors · `playbooks` |
| Embedding | Titan Text Embeddings V2, float32, 1.024 chiều, cosine |
| Nguồn tài liệu và minh chứng | Prefix `playbooks/` trong bucket KB docs. Lần ingestion gần nhất COMPLETE, kết quả Retrieve dẫn về một tài liệu trong prefix. |

3. Ghi `KnowledgeBaseId` khi script báo `GATE: PHISHGUARD_KB_RETRIEVE_PASS`. Trong **Bedrock → Guardrails**, tạo guardrail với bộ lọc **Prompt attacks** trên input, đặt hành vi chặn, tạo phiên bản và ghi ID/version. Trong Model catalog xác nhận tài khoản có thể gọi Nova Lite v1 hoặc đúng inference profile được cấp cho vùng. Nếu dùng inference profile, điền `ModelId` của profile và `ModelInvokeResourceArns` gồm chính xác các ARN cần cho profile và model đích.
4. Cập nhật backend, sau đó chạy `DEMO_2` để xem phần tổng hợp. Khi gặp `FALLBACK`, đọc `fallback_reason`, kiểm tra trạng thái đồng bộ dữ liệu, ID/version Guardrail và quyền Retrieve/InvokeModel của Synthesize trước khi kết luận model đã chạy thành công.

```powershell
$kb = Read-Host 'KnowledgeBaseId'
$gr = Read-Host 'GuardrailId'
$gv = Read-Host 'GuardrailVersion da phat hanh'
Update-PgStack @{KnowledgeBaseId=$kb; GuardrailId=$gr; GuardrailVersion=$gv}
```

### Kiểm tra sáu playbook đã lập chỉ mục

Trong S3, mở bucket do `KbDocsBucketName` trả về và prefix `playbooks/`. Ba tài liệu đầu là sổ tay SecOps, xác minh chuyển khoản và dấu hiệu lừa đảo. File 04 hướng dẫn đọc email chuyển tiếp và DMARC. File 05 hướng dẫn xem xét cảnh báo HIGH thiếu mục tiêu hành động. File 06 hướng dẫn xác minh lời mời nhận trợ cấp có hạn chót.

Ảnh lịch sử bên dưới ghi nhận năm tài liệu tại lần sync trước khi có 06. Khi chạy một sync mới, ghi mã job và số tài liệu lỗi. Kiểm tra riêng khả năng truy xuất trong **Test knowledge base** bằng ba câu hỏi dưới đây. Công cụ đi kèm kiểm tra ba nguồn đầu.

```powershell
.\.venv\Scripts\python.exe .\scripts\check_playbooks.py --kb $kb --profile $pgProfile
```

| Câu hỏi thử | Nguồn cần tìm và cách đọc |
|---|---|
| Return-Path trong EML khác From. Có đủ căn cứ kết luận DMARC của thư gốc thất bại không? | Tìm playbook 04. Phân biệt xác thực của email lớp ngoài với header do người gửi chuyển tiếp cung cấp. |
| Sự cố HIGH thiếu domain đủ căn cứ để chặn. Chuyên viên nên làm gì tiếp? | Tìm playbook 05. Đối chiếu các chỉ dấu và chuyển kiểm tra thủ công. |
| SMS mời nhận trợ cấp qua liên kết và gây áp lực hết hạn. Nên xác minh thế nào? | Tìm `06-nhan-tro-cap-qua-lien-ket.txt`. Đối chiếu hướng dẫn với luật và đề xuất trong incident. |

Ghi lại tên file và đoạn nguồn trả về. Sau đó mở H05/H11 trên portal, xem nguồn trích thực tế và lý do fallback nếu có. Kết quả của câu hỏi thử KB và câu truy vấn do ứng dụng tự tạo có thể khác nhau.

![Trước khi thêm playbook 06, lượt đồng bộ quét 5 tài liệu, thêm 2, lỗi 0.]({{< asset "images/phishguard/kb-sync.png" >}})

Với stack đang chạy, xem **Knowledge Bases → Data sources → Documents** và đối chiếu cả URI lẫn trạng thái. Nếu tài liệu đã nạp qua Console và đúng URI là `INDEXED`, tiếp tục thử Retrieve. Nếu mới tải file vào S3 và chỉ mục chưa có bản đó, chọn Sync trên đúng data source và đợi `COMPLETE`.

Trong lần kiểm tra trên AWS, cả 6/6 playbook đều ở trạng thái `INDEXED`. Khi hỏi trực tiếp cách xử lý SMS yêu cầu bấm liên kết nhận trợ cấp BHTN trước hạn, Retrieve trả về playbook 04, 06, 05 theo thứ tự. Synthesize của incident SMS `INC-9c67814b7c0a` thực tế trích playbook 03 và 01. [Kết quả của ba incident trên AWS](../../verify/demo/#aws-results).

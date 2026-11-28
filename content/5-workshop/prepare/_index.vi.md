---
title: "5.2 · Chuẩn bị môi trường"
weight: 2
chapter: false
aliases:
  - /vi/5-workshop/02-prerequisites/
---

Dùng một tài khoản lab mà người triển khai được phép quản lý, chọn **US East (N. Virginia), us-east-1**. Tên vật lý của một số Lambda, bảng, user pool group và IAM role được cố định trong mẫu. Khi học lại trên tài khoản đã có PhishGuard, bắt đầu từ phần đối chiếu của stack hiện tại. Để dựng độc lập từ đầu, dùng một tài khoản lab riêng.

- Cài Git, Python 3.12 x64, Node.js 22 LTS, AWS CLI v2 và AWS SAM CLI. Docker chỉ cần cho phương án build trong container Linux. Người triển khai cần quyền quản lý stack, đóng gói S3 và triển khai các tài nguyên trong template, gồm quyền chuyển đúng role cho dịch vụ. Người phụ trách Billing cần đọc Bills, Cost Explorer và quản lý Budgets.
- Chuẩn bị quyền chỉnh DNS trên một domain hoặc subdomain nhận thư. Dùng ba địa chỉ kiểm thử do nhóm sở hữu, riêng cho reporter, admin, auditor; thêm một tài khoản reporter thứ hai để kiểm thử phân tách người dùng.
- Cấu hình profile AWS tên `phishguard` bằng cơ chế đăng nhập của tài khoản. Profile này chọn danh tính AWS của người triển khai. Các tài khoản `demo-*` đăng nhập ứng dụng bằng Cognito.
- Giải nén [PhishGuard-Lab.zip]({{< asset "downloads/PhishGuard-Lab.zip" >}}). Thư mục `lab/` chứa mã thực hành, `tools/` chứa công cụ đọc cấu hình. Các đoạn PowerShell phía dưới dùng chung một cửa sổ terminal trong `lab/`.

```powershell
Set-Location .\lab
$pgProfile = 'phishguard'
$pgRegion = 'us-east-1'
$pgStack = 'phishguard-ai'
aws sts get-caller-identity --profile $pgProfile --query '{Account:Account,Arn:Arn}'
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements-dev.txt
sam --version
node --version
Test-Path .\layers\ocr-vie-py312-x86_64.zip
```

**Mốc kiểm tra.** Account phải là tài khoản lab được chọn, Python là 3.12, `Test-Path` trả `True`. Nếu profile dùng IAM Identity Center và phiên đã hết hạn, chạy `aws sso login --profile phishguard`.

Trong cùng cửa sổ, kiểm tra layer QR và tài liệu đi kèm bộ lab đầy đủ. Kết quả mong đợi là True và sáu file. Nếu dùng profile có tên khác, đặt $pgProfile theo đúng tên đã đăng nhập.

```powershell
Test-Path .\layers\qr-py312-x86_64.zip
(Get-ChildItem .\kb-docs\playbooks\*.txt).Count
```

## Các công cụ kiểm tra trong gói lab

Một số tên file được giữ từ các đợt phát triển trước. Bảng sau giải thích chức năng để chọn đúng công cụ; các lệnh trong workshop dùng chính những tên file này.

| File trong `scripts/` | Chức năng |
|---|---|
| `phase2b_smoke.py` | Kiểm tra nhanh API, đăng nhập, phân quyền, gửi lặp và một báo cáo vô hại sau khi triển khai backend. |
| `verify_group1_upload.py` | Kiểm tra presigned upload, giới hạn tệp, OCR và ảnh trắng. |
| `verify_group2_samples.py` | Kiểm tra các mẫu email DEMO_2/3 và những nhánh xử lý liên quan. |
| `verify_admin_api.py` | Kiểm tra quyền admin/auditor, truy cập raw evidence và quyết định phê duyệt. |
| `check_phase_h_local.py` | Chạy các hồi quy H bằng mô phỏng AWS trên máy, gồm H04 và phần lỗi giữa chừng của H13. |
| `check_phase_h_live.py` | Kiểm tra H01–H03 trên AWS hoặc quyết định muộn H14 với tùy chọn `--expired`. |
| `check_playbooks.py` | Truy vấn KB và kiểm tra nguồn playbook 04/05/06 trong kết quả. |

Thông báo như `GATE: PHASE_2B_SMOKE_PASS` là nhãn kết quả của script. Nó chỉ xác nhận những phép kiểm script vừa chạy, không xác nhận toàn bộ hệ thống. Các kết quả `SKIPPED` và `UNVERIFIED` giữ nguyên ý nghĩa chưa thực hiện hoặc chưa xác minh.

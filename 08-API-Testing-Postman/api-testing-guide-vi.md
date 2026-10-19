# Hướng dẫn API Testing Spa POS bằng Postman (VN)

> **Nguồn:** Tổng hợp từ các cuộc thực hành 8–14/10/2026 và tài liệu QA hiện có. Đây là hướng dẫn tái dựng, **không phải bản export gốc hoặc kết quả chạy lại**.

## 1. Chuẩn bị

- Cài Postman và dùng **Desktop Agent** nếu Cloud Agent không gửi được request.
- Import [Collection JSON](Spa-POS-QA.postman_collection.json) và [Environment JSON](Spa-POS-QA.postman_environment.json) vào Postman; chọn environment.
- Trong **environment trên máy cá nhân**, điền `SUPABASE_URL`, `SUPABASE_API_KEY` (publishable/anon), `ADMIN_EMAIL`, `ADMIN_PASSWORD`. Không điền service-role key; không commit environment đã có giá trị thật.
- Chạy Login bằng test account được phép; sao chép `access_token` vào biến `ACCESS_TOKEN` chỉ trong Postman local. Không chụp/đăng JWT.
- Đặt `TEST_INVOICE_ID` và `TEST_SHIFT_ID` thành **fixture thử nghiệm mới**, không tái sử dụng ID hóa đơn lịch sử vì có thể đã thay đổi.
- Với v2, tự tạo UUID mới cho `IDEMPOTENCY_KEY` (ví dụ qua công cụ UUID đáng tin cậy). Giữ nguyên key để thử retry, thay key khi cần một giao dịch mới.

## 2. Request, header và body

**Login:** `POST {{SUPABASE_URL}}/auth/v1/token?grant_type=password`

```json
{
  "email": "{{ADMIN_EMAIL}}",
  "password": "{{ADMIN_PASSWORD}}"
}
```

**RPC:** `POST {{SUPABASE_URL}}/rest/v1/rpc/<function_name>`

Headers:

```text
apikey: {{SUPABASE_API_KEY}}
Authorization: Bearer {{ACCESS_TOKEN}}
Content-Type: application/json
```

`{{...}}` là biến Postman, không phải giá trị literal gửi nguyên lên backend.

## 3. Các luồng từng thực hành

| Nghiệp vụ | Endpoint | Dữ liệu minh họa | Kết quả đã ghi nhận |
| --- | --- | --- | --- |
| Đăng nhập | `/auth/v1/token?grant_type=password` | Email/password test | HTTP 200 |
| Mở ca | `/rest/v1/rpc/open_shift` | `p_opening_cash: 1000000` | 200, ca OPEN |
| Mở ca trùng | `open_shift` | Khi ca đang OPEN | 409 / 23505 |
| Mở ca âm | `open_shift` | `p_opening_cash: -1` | 400 / 22023 |
| Lấy ca hiện tại | `get_current_shift` | `{}` | 200 được ghi nhận |
| Đóng ca | `close_shift` | `p_shift_id`, `p_actual_cash: 900000` | 200; chênh lệch -100000 trong tình huống lịch sử |
| Đóng ca CLOSED | `close_shift` | Ca đã CLOSED | Trước fix 500/55000; sau fix 409/23505 |
| Thanh toán đủ/một phần | `create_payment` (legacy) | 110000 hoặc 60000 | PAID / PARTIALLY_PAID; RPC legacy có thể đã bị thu hồi quyền |
| Thanh toán v2 | `create_payment_v2` | Thêm `p_idempotency_key` | Same key/same payload không tạo trùng; same key/khác payload 409 |
| Hủy hóa đơn không tồn tại | `cancel_invoice` | `p_invoice_id: 999999` | Sau fix 400 / 22023 / INVOICE_NOT_FOUND |

**Quan trọng:** `create_payment` legacy đã có ghi nhận thu hồi EXECUTE của `anon/authenticated`. Các request legacy là **minh họa lịch sử**, không cam kết còn gọi thành công trên backend hiện tại. Kiểm tra function signature/quyền trước khi chạy.

## 4. Kiểm tra response

- **HTTP 200:** request thành công (không thay thế đối chiếu DB).
- **400:** đầu vào hoặc nghiệp vụ không hợp lệ, tùy SQLSTATE.
- **401:** thiếu/hết hạn authentication (cần kiểm tra chính xác response).
- **403:** tài khoản không đủ quyền, ví dụ `42501`.
- **409:** conflict, ví dụ `23505`, `SHIFT_NOT_OPEN` hoặc `IDEMPOTENCY_KEY_CONFLICT`.
- **500:** lỗi phía server; đã từng gặp mapping chưa đúng khi đóng ca CLOSED.

Các nhóm HTTP: 1xx thông tin; 2xx thành công; 3xx điều hướng; 4xx lỗi request/quyền; 5xx lỗi server. SQLSTATE không đồng nghĩa trực tiếp với HTTP status; mapping do API/backend quyết định.

## 5. Kiểm tra database và bug report

Sau request, kiểm tra SQL read-only trong [SQL Verification](sql-verification-queries.md). Với request thất bại, kiểm tra **không có payment/cashbook/invoice mutation ngoài ý muốn**. Chỉ nhìn HTTP 403/400 chưa đủ kết luận tính toàn vẹn dữ liệu.

Bug report ghi: Defect ID, môi trường, precondition, bước tái hiện, request body (đã ẩn bí mật), expected/actual, HTTP/SQLSTATE, dữ liệu DB trước/sau, severity/priority đề xuất, evidence, trạng thái retest.

## 6. Phân biệt kết quả và bài tập chưa hoàn thành

- **Đã ghi nhận lịch sử:** TC-SHIFT, TC-PAYMENT, cancel_invoice retest, một phần DEF-SALE-013.
- **Chưa chạy/chưa xác minh đầy đủ:** direct call legacy RPC denial, lost-response/timeout retry, invoice creation idempotency, một số post-request integrity checks.
- **Evidence còn thiếu:** Postman screenshots/exports đã ẩn thông tin nhạy cảm, SQL result theo từng test case, build SHA.

Tham chiếu: [Postman Test Cases](../03-Test-Cases/postman-api-executions.md), [Reviewed API Evidence](qa-execution-reviewed-en.md), [Payment Retest](../03-Test-Cases/def-sale-013-retest-2026-10-14.md).

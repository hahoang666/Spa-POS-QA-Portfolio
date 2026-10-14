# Kiểm thử E2E — Bán hàng → Hóa đơn → Thanh toán → Sổ quỹ

**Ứng dụng:** [Spa POS](https://hahoang666.github.io/Spa-pos-pp/)  
**Tài liệu yêu cầu:** [REQUIREMENT_BASELINE_V1.0](https://github.com/hahoang666/Spa-pos-pp/blob/main/docs/REQUIREMENT_BASELINE_V1.0.md) (SALE-02, SALE-04, SALE-05, SALE-06)  
**Ngày kiểm thử:** 14/10/2026  
**Build / commit:** Chưa ghi nhận  
**Evidence:** Kết quả quan sát giao diện và SQL Supabase do tester cung cấp trong phiên kiểm thử; chưa commit ảnh chụp.

## Danh sách test case

| Test Case ID | Requirement ID | Loại kiểm thử | Kịch bản | Bước thực hiện / dữ liệu thử | Kết quả mong đợi | Ưu tiên | Kết quả thực tế | Trạng thái | Defect ID | Evidence | Build / Ngày |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| SALE-E2E-01 | SALE-02, SALE-04, SALE-05, SALE-06 | Functional / Integration / E2E | Thanh toán đủ tiền mặt, ghi nhận hóa đơn và phiếu thu đúng, dữ liệu tồn tại sau tải lại, không trùng bản ghi | Ca thu ngân đã mở; bán 1 × DV000001, không giảm giá, VAT 10%; thanh toán CASH 110.000 ₫; tải lại trang; đối soát invoices, invoice_items, payments, cashbook_transactions và số lượng bản ghi | Hóa đơn COMPLETED/PAID; trước VAT 100.000, VAT 10.000, tổng/đã thu 110.000, còn nợ 0; 1 dòng chi tiết, 1 thanh toán CASH, 1 khoản thu IN vào quỹ CASH; dữ liệu còn sau reload; không trùng | Critical | HD00000010 (id 10) COMPLETED/PAID, trước VAT 100000, VAT 10000, tổng/đã thu 110000, nợ 0. Chi tiết id 10: DV000001, SL 1, tổng 110000. Payment id 9: CASH 110000, ca 4. Cashbook id 10: quỹ 1, IN 110000, payment_id 9, tham chiếu INVOICE/10. Giao diện vẫn đúng sau Ctrl+Shift+R. Số lượng: chi tiết=1, thanh toán=1, sổ quỹ=1. | PASS | — | Tester cung cấp kết quả giao diện và SQL; chưa commit ảnh | 14/10/2026; chưa ghi nhận build |
| SALE-E2E-02 | SALE-02, SALE-04, SALE-05 | Functional / Integration / E2E | Thanh toán đủ bằng chuyển khoản, ghi đúng quỹ ngân hàng | Ca 4 đã mở; bán 1 × DV000001, VAT 10%, không giảm giá; thanh toán BANK_TRANSFER 110000; đối soát invoices, invoice_items, payments, cashbook_transactions, funds; tải lại và kiểm tra số bản ghi | PAID, nợ 0; một thanh toán BANK_TRANSFER, một phiếu thu IN đúng quỹ BANK_TRANSFER; không ghi CASH; dữ liệu tồn tại và không trùng | High | HD00000011 (invoice id 11): COMPLETED/PAID, trước VAT 100000, VAT 10000, tổng/đã thu 110000, nợ 0. Chi tiết id 11: DV000001, SL 1, tổng 110000. Payment id 10: BANK_TRANSFER 110000, ca 4. Cashbook id 11: quỹ 2 BANK_TRANSFER, IN 110000, payment_id 10, INVOICE/11. Đối soát quỹ: BANK_TRANSFER 1 dòng, tổng 110000; không có dòng CASH. Sau reload hóa đơn và phiếu thu ngân hàng vẫn hiển thị. Số lượng chi tiết=1, thanh toán=1, sổ quỹ=1. | PASS | — | Tester cung cấp kết quả giao diện, 5 truy vấn SQL và kết quả sau reload; chưa commit ảnh | 14/10/2026; chưa ghi nhận build |
| SALE-E2E-03 | SALE-02, SALE-04, SALE-05 | Functional / Integration / E2E | Thanh toán một phần bằng CASH, giữ đúng công nợ và chỉ ghi quỹ tiền thực thu | Ca 4; bán 1 × DV000001, VAT 10%, không giảm giá; thu CASH 70000 trên tổng 110000; đối soát hóa đơn, chi tiết, thanh toán, sổ quỹ; tải lại và đếm bản ghi | Hóa đơn COMPLETED/PARTIALLY_PAID, tổng 110000, đã thu 70000, còn nợ 40000; một thanh toán CASH và một khoản thu IN quỹ CASH chỉ 70000; dữ liệu tồn tại, không trùng | High | HD00000012 (invoice id 12): COMPLETED/PARTIALLY_PAID, trước VAT 100000, VAT 10000, tổng 110000, đã thu 70000, nợ 40000. Chi tiết id 12: DV000001 × 1, tổng 110000. Payment id 11: CASH 70000, ca 4. Cashbook id 12: quỹ 1 CASH, IN 70000, payment_id 11, INVOICE/12. Sau reload hóa đơn và phiếu thu vẫn đúng. Số lượng chi tiết=1, thanh toán=1, sổ quỹ=1. | PASS | — | Tester cung cấp kết quả giao diện, 4 truy vấn SQL và kết quả sau reload; chưa commit ảnh | 14/10/2026; chưa ghi nhận build |
| SALE-E2E-04 | SALE-04, SALE-05, SALE-06 | Functional / Integration / E2E | Thu nợ còn lại bằng chuyển khoản | Hóa đơn HD00000012 đã thu CASH 70000, nợ 40000; chọn Thu nợ BANK_TRANSFER 40000, xác nhận, tải lại, đối soát invoices/payments/cashbook | Hóa đơn PAID, đã thu 110000, nợ 0; có payment BANK_TRANSFER 40000 và phiếu thu IN quỹ ngân hàng 40000; dữ liệu tồn tại | High | Retest 14/10/2026: UI báo đồng bộ Supabase thành công; sau reload hóa đơn PAID, nợ 0, ẩn nút Thu nợ, quỹ ngân hàng tăng 40000. SQL: invoice id 12 tổng/đã thu 110000, nợ 0; payment id 11 CASH 70000 và id 12 BANK_TRANSFER 40000; cashbook id 12 CASH IN 70000 và id 13 BANK_TRANSFER IN 40000, cùng liên kết invoice 12. Lịch sử FAIL giữ ở phần dưới. | PASS (RETEST) | DEF-SALE-004 | UI sau reload và 3 kết quả SQL Supabase; merge commit a3865a1 | 14/10/2026; chưa ghi nhận build |

## SALE-E2E-01 — Bằng chứng đối soát SQL

| Bảng | Khóa liên kết | Kết quả thực tế |
| --- | --- | --- |
| `public.invoices` | `id=10`, `code=HD00000010` | `status=COMPLETED`, `payment_status=PAID`, `subtotal_before_vat=100000`, `vat_amount=10000`, `discount_amount=0`, `total_amount=110000`, `paid_amount=110000`, `due_amount=0` |
| `public.invoice_items` | `id=10`, `invoice_id=10` | `product_code=DV000001`, `quantity=1`, `unit_price_before_vat=100000`, `unit_price_after_vat=110000`, `line_subtotal_before_vat=100000`, `line_vat_amount=10000`, `line_total_amount=110000` |
| `public.payments` | `id=9`, `invoice_id=10` | `payment_method_code=CASH`, `amount=110000`, `shift_id=4` |
| `public.cashbook_transactions` | `id=10`, `payment_id=9` | `fund_id=1`, `direction=IN`, `transaction_type=SALE`, `amount=110000`, `reference_type=INVOICE`, `reference_id=10`, `shift_id=4` |
| Kiểm tra lưu dữ liệu / trùng lặp | Sau Ctrl+Shift+R | Hóa đơn và phiếu thu vẫn hiển thị; SQL: `item_count=1`, `payment_count=1`, `cashbook_count=1` |

**Kết luận:** PASS cho kịch bản bán một dịch vụ và thanh toán đủ bằng CASH đã thực hiện. Chưa kiểm thử các phương thức khác, tình huống lỗi, đồng thời và cơ chế chống trùng lặp trong các điều kiện khác.

**Lưu ý SQL:** Hai truy vấn ban đầu tham chiếu nhầm cột `invoice_items.subtotal_before_vat` và `cashbook_transactions.type` (PostgreSQL 42703). Đây là lỗi câu lệnh kiểm thử, không phải defect ứng dụng. Truy vấn sửa theo schema thực tế đã trả kết quả như trên.

## SALE-E2E-02 — Bằng chứng đối soát SQL

| Bảng | Khóa liên kết | Kết quả thực tế |
| --- | --- | --- |
| `public.invoices` | `id=11`, `code=HD00000011` | `status=COMPLETED`, `payment_status=PAID`, `subtotal_before_vat=100000`, `vat_amount=10000`, `discount_amount=0`, `total_amount=110000`, `paid_amount=110000`, `due_amount=0` |
| `public.invoice_items` | `id=11`, `invoice_id=11` | `product_code=DV000001`, `quantity=1`, `unit_price_before_vat=100000`, `unit_price_after_vat=110000`, `vat_rate=10`, `line_subtotal_before_vat=100000`, `line_discount_amount=0`, `line_vat_amount=10000`, `line_total_amount=110000` |
| `public.payments` | `id=10`, `invoice_id=11` | `payment_method_id=2`, `payment_method_code=BANK_TRANSFER`, `amount=110000`, `shift_id=4` |
| `public.cashbook_transactions` | `id=11`, `payment_id=10` | `fund_id=2`, `fund_code=BANK_TRANSFER`, `shift_id=4`, `transaction_type=SALE`, `direction=IN`, `amount=110000`, `reference_type=INVOICE`, `reference_id=11` |
| Đối soát quỹ | `HD00000011` | Một dòng `BANK_TRANSFER`: `transaction_count=1`, `total_amount=110000`; không có dòng `CASH` |
| Persistence / Duplicate | Sau Ctrl+Shift+R | Hóa đơn và phiếu thu ngân hàng vẫn hiển thị; SQL: `item_count=1`, `payment_count=1`, `cashbook_count=1` |

**Kết luận:** PASS cho kịch bản thanh toán đủ bằng chuyển khoản đã thực hiện. Các trường hợp thanh toán thiếu, nhiều phương thức và lỗi vẫn chưa kiểm thử.

## SALE-E2E-03 — Bằng chứng SQL thanh toán một phần

| Bảng | Khóa liên kết | Kết quả thực tế |
| --- | --- | --- |
| `public.invoices` | `id=12`, `code=HD00000012` | `status=COMPLETED`, `payment_status=PARTIALLY_PAID`, `subtotal_before_vat=100000`, `vat_amount=10000`, `discount_amount=0`, `total_amount=110000`, `paid_amount=70000`, `due_amount=40000` |
| `public.invoice_items` | `id=12`, `invoice_id=12` | `product_code=DV000001`, `quantity=1`, `unit_price_before_vat=100000`, `unit_price_after_vat=110000`, `vat_rate=10`, `line_total_amount=110000` |
| `public.payments` | `id=11`, `invoice_id=12` | `payment_method_id=1`, `payment_method_code=CASH`, `amount=70000`, `shift_id=4` |
| `public.cashbook_transactions` | `id=12`, `payment_id=11` | `fund_id=1`, `fund_code=CASH`, `transaction_type=SALE`, `direction=IN`, `amount=70000`, `reference_type=INVOICE`, `reference_id=12`, `shift_id=4` |
| Persistence / Duplicate | Sau Ctrl+Shift+R | Hóa đơn vẫn đã thu 70000, còn nợ 40000; phiếu thu 70000 vẫn hiển thị; `item_count=1`, `payment_count=1`, `cashbook_count=1` |

**Kết luận:** PASS cho kịch bản thanh toán một phần bằng CASH đã thực hiện. Chưa kiểm thử thu nợ 40000 còn lại và các tình huống lỗi hoặc phương thức khác.


## SALE-E2E-04 — Lịch sử lỗi và Retest (14/10/2026)

- **FAIL ban đầu / DEF-SALE-004:** UI báo thu nợ thành công sai; sau reload invoice 12 vẫn PARTIALLY_PAID, đã thu 70000, nợ 40000. Chỉ có payment 11 và cashbook 12 (CASH 70000). Nguyên nhân: hàm `collectDebt` chỉ cập nhật state cục bộ.
- **Bản sửa:** PR #2 đã merge vào `main`, squash commit `a3865a175ab25b1f9462ddf66877192d35f397f2`; frontend gọi Supabase `create_payment` và đồng bộ lại dữ liệu.
- **RETEST PASS:** Invoice 12 `COMPLETED/PAID`, tổng 110000, đã thu 110000, nợ 0; payment 11 CASH 70000 + payment 12 BANK_TRANSFER 40000; cashbook 12 CASH IN 70000 + cashbook 13 BANK_TRANSFER IN 40000, cùng tham chiếu invoice 12. UI giữ trạng thái sau reload; ẩn nút Thu nợ; quỹ ngân hàng tăng 40000.
- **Trạng thái defect:** DEF-SALE-004 VERIFIED / CLOSED trong phạm vi kịch bản đã báo. Regression và nhánh lỗi khác chưa kiểm thử.


## REG-SALE-05 — Chặn thu nợ vượt công nợ (14/10/2026)

- **Kết quả:** PASS (UI regression và đối soát dữ liệu); chưa kiểm tra API trực tiếp.
- **Dữ liệu:** HD00000008 (invoice id 8), PARTIALLY_PAID, đã thu 70000, còn nợ 40000.
- **Thao tác:** Nhập 40001, chọn chuyển khoản và xác nhận một lần. UI cảnh báo “Số tiền thu không hợp lệ.”; công nợ vẫn 40000.
- **SQL trước và sau:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — không thay đổi.
- **Quan sát Console:** `index.ts.js:5 {error: "SOURCE_LANG_VI"}`; chưa xác định liên quan ứng dụng hay tiện ích trình duyệt, không tính là defect của luồng này.
- **Giới hạn:** Chưa xác minh backend tự chặn overpayment khi gọi RPC/API trực tiếp.


## REG-SALE-06 — Chặn thu nợ bằng 0 và số âm (14/10/2026)

- **Kết quả:** PASS (UI validation và đối soát dữ liệu); chưa kiểm thử API trực tiếp.
- **Hóa đơn:** HD00000008, invoice id 8, còn nợ 40000.
- **Thao tác:** Nhập `0` và `-1000` vào số tiền thu, lần lượt nhấn Thu nợ. Cả hai lần UI đều hiển thị “Số tiền thu không hợp lệ.”; công nợ vẫn 40000.
- **SQL sau kiểm thử so với baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — không thay đổi.
- **Giới hạn:** Chưa chứng minh RPC `create_payment` tự từ chối số tiền không hợp lệ nếu gọi trực tiếp.


## REG-SALE-07 — API chặn thanh toán vượt công nợ (14/10/2026)

- **Kết quả:** PASS — Postman RPC và đối soát SQL.
- **Dữ liệu:** HD00000008, invoice id 8, còn nợ 40000.
- **Request:** `POST /rest/v1/rpc/create_payment` với `p_invoice_id=8`, `p_payment_method_id=2`, `p_amount=40001`, `p_reference_no=null`, `p_notes=REG-SALE-07-OVERPAYMENT`.
- **Response:** HTTP `400 Bad Request`, PostgreSQL code `22023`, message `Payment amount exceeds invoice due amount`.
- **SQL sau API so với baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — không thay đổi.
- **Kết luận:** Backend từ chối overpayment và không ghi nhận giao dịch mới trong kịch bản này. Các trường hợp API bằng 0, số âm hoặc lỗi khác chưa kiểm thử.


## REG-SALE-08 — API từ chối thanh toán bằng 0 (14/10/2026)

- **Kết quả:** PASS — Postman RPC và đối soát SQL.
- **Dữ liệu:** HD00000008, invoice id 8, công nợ 40000.
- **Request:** `POST /rest/v1/rpc/create_payment` với `p_invoice_id=8`, `p_payment_method_id=2`, `p_amount=0`, `p_notes=REG-SALE-08-ZERO-AMOUNT`.
- **Response:** HTTP `400 Bad Request`, code `22023`, message `Payment amount must be greater than 0`.
- **SQL sau API so với baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — không thay đổi.
- **Kết luận:** Backend từ chối giá trị 0 và không tạo giao dịch mới. Kiểm thử API với số âm chưa thực hiện.


## REG-SALE-09 — API từ chối thanh toán số âm (14/10/2026)

- **Kết quả:** PASS — Postman RPC và đối soát SQL.
- **Dữ liệu:** HD00000008, invoice id 8, công nợ 40000.
- **Request:** `POST /rest/v1/rpc/create_payment`, `p_invoice_id=8`, `p_payment_method_id=2`, `p_amount=-1000`, `p_notes=REG-SALE-09-NEGATIVE-AMOUNT`.
- **Response:** HTTP `400 Bad Request`, code `22023`, message `Payment amount must be greater than 0`.
- **SQL sau API so với baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — không thay đổi.
- **Tổng hợp Regression:** REG-SALE-05, REG-SALE-06, REG-SALE-07, REG-SALE-08, REG-SALE-09: **5/5 PASS** trong phạm vi đã kiểm thử.
- **Giới hạn:** Chưa kiểm thử race condition, API lỗi hệ thống, đồng thời nhiều request hoặc tất cả nhánh quyền.


## REG-SALE-10 — Phân quyền API create_payment (14/10/2026)

- **Kết quả:** PASS — kiểm tra phân quyền và đối soát SQL.
- **Quyền:** `PAYMENT_CREATE`: ADMIN = GRANTED; CASHIER = NOT_GRANTED; EMPLOYEE = NOT_GRANTED (SQL đối chiếu `roles`, `permissions`, `role_permissions`).
- **Tài khoản thử:** `employee01` (EMPLOYEE), gọi `POST /rest/v1/rpc/create_payment` với `p_invoice_id=8`, `p_payment_method_id=2`, `p_amount=1000`, `p_notes=REG-SALE-10-UNAUTHORIZED`.
- **Response:** HTTP `403 Forbidden`, PostgreSQL code `42501`, message `FORBIDDEN: missing permission PAYMENT_CREATE`.
- **SQL sau request so với baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — không đổi.
- **Kết luận:** EMPLOYEE không thể tạo thanh toán trái quyền và không ghi dữ liệu. **Cần rà soát yêu cầu nghiệp vụ CASHIER** vì vai trò này cũng đang thiếu `PAYMENT_CREATE`; chưa kết luận đó là defect nếu chưa đối chiếu baseline.


## REG-SALE-11 — Phân quyền thanh toán CASHIER (14/10/2026)

- **Kết quả:** FAIL so với nghiệp vụ thu tiền của CASHIER; toàn vẹn dữ liệu giao dịch bị từ chối PASS.
- **Điều kiện:** CASHIER chỉ có ATTENDANCE_SELF, RETURN_CREATE, RETURN_VIEW, shift.open, shift.close, shift.view. INVOICE_CREATE, INVOICE_VIEW, PAYMENT_CREATE tồn tại nhưng chưa gán cho CASHIER.
- **Thực thi:** CASHIER gọi `POST /rest/v1/rpc/create_payment` cho hóa đơn id=8, phương thức 2, số tiền 1000, ghi chú REG-SALE-11-CASHIER-AUTH.
- **Actual:** HTTP 403, code 42501, `FORBIDDEN: missing permission PAYMENT_CREATE`.
- **SQL sau request:** HD00000008 vẫn PARTIALLY_PAID, đã thu 70000, còn nợ 40000; payment_count=1, payment_sum=70000; cashbook_count=1, cashbook_in_sum=70000 (không đổi so với baseline).
- **Defect:** DEF-SALE-011, OPEN; nghi vấn cấu hình gán quyền sai nghiệp vụ, Severity Major, Priority High (đề xuất). API từ chối đúng theo quyền hiện hành nhưng luồng thu tiền CASHIER bị chặn. Cần xác nhận ánh xạ role và phạm vi thanh toán/thu nợ trước khi sửa. Chưa thay đổi quyền.


### RETEST-SALE-011-01 — CASHIER thanh toán sau migration (14/10/2026)

- **Kết quả: PASS (API và SQL).** CASHIER được gán INVOICE_CREATE, INVOICE_VIEW, PAYMENT_CREATE; SQL xác nhận tổng 9 quyền, giữ nguyên 6 quyền cũ.
- **API:** POST /rest/v1/rpc/create_payment với invoice_id=8, payment_method_id=2, amount=1000, ghi chú RETEST-SALE-011-01-CASHIER trả HTTP 200. Tạo payment id=13, shift_id=6.
- **SQL:** HD00000008 vẫn PARTIALLY_PAID; paid_amount=71000 (trước 70000), due_amount=39000 (trước 40000); payment_count=2, payment_sum=71000; cashbook_count=2, cashbook_in_sum=71000 (trước đó đều 1/70000).
- **DEF-SALE-011:** FIXED / chờ xác minh cuối; chưa chạy thu nợ hóa đơn cũ bằng CASHIER và regression quyền EMPLOYEE. Giữ kết quả REG-SALE-11 FAIL ban đầu làm bằng chứng lịch sử.


### RETEST-SALE-011-02 — CASHIER thu nợ hóa đơn cũ qua UI (14/10/2026)

- **Kết quả: PASS (UI và SQL).** Đăng nhập CASHIER, thấy hóa đơn HD00000008 và nút Thu nợ. Thu thêm 1.000 ₫ qua giao diện Spa POS; UI báo đồng bộ Supabase và không có lỗi Console.
- **SQL trước → sau:** PARTIALLY_PAID → PARTIALLY_PAID; paid_amount 71000 → 72000; due_amount 39000 → 38000; payment_count 2 → 3; payment_sum 71000 → 72000; cashbook_count 2 → 3; cashbook_in_sum 71000 → 72000.
- **Đối soát:** 72000 + 38000 = tổng hóa đơn 110000; payment sum = cashbook IN sum.
- **DEF-SALE-011:** FIXED / chờ REG-SALE-12 kiểm thử hồi quy quyền EMPLOYEE trước khi CLOSED. Giữ REG-SALE-11 FAIL ban đầu.


### REG-SALE-12 — Hồi quy phân quyền EMPLOYEE (14/10/2026)

- **Trạng thái: PASS.** SQL phân quyền: PAYMENT_CREATE được cấp cho ADMIN và CASHIER, không cấp cho EMPLOYEE.
- **Postman:** Đăng nhập employee01 (EMPLOYEE), gọi POST /rest/v1/rpc/create_payment cho hóa đơn 8, phương thức 2, số tiền 1000, ghi chú REG-SALE-12-EMPLOYEE-DENIED. Kết quả HTTP 403 Forbidden, mã 42501, thông báo "FORBIDDEN: missing permission PAYMENT_CREATE".
- **Đối soát SQL:** HD00000008 giữ nguyên PARTIALLY_PAID; paid_amount=72000, due_amount=38000, payment_count=3, payment_sum=72000, cashbook_count=3, cashbook_in_sum=72000. Không phát sinh giao dịch trái quyền.

### DEF-SALE-011 — CLOSED (Verified Fixed, 14/10/2026)

- **Lỗi ban đầu:** CASHIER không thể thu tiền do thiếu PAYMENT_CREATE, API trả HTTP 403; REG-SALE-11 FAIL.
- **Khắc phục:** Cấp INVOICE_CREATE, INVOICE_VIEW, PAYMENT_CREATE cho CASHIER; SQL xác nhận tổng 9 quyền, giữ nguyên quyền cũ.
- **Retest:** RETEST-SALE-011-01 PASS (thu tiền qua API HTTP 200 và đối soát Invoice/Payment/Cashbook); RETEST-SALE-011-02 PASS (thu nợ qua giao diện và đối soát SQL).
- **Regression:** REG-SALE-12 PASS: EMPLOYEE vẫn không được thanh toán, dữ liệu tài chính không thay đổi.
- **Trạng thái cuối:** CLOSED / Verified Fixed. Severity: Major; Priority: High.


### REG-SALE-13 — Thanh toán trùng khi gửi lại cùng request API (14/10/2026)

- **Kết quả: FAIL.** Quy tắc nghiệp vụ đã xác nhận: cùng một yêu cầu thanh toán gửi lại không được tạo giao dịch thứ hai; hai yêu cầu thanh toán độc lập vẫn được phép khi đáp ứng điều kiện thanh toán.
- **Baseline HD00000008 (id=8):** paid=72000, due=38000, payment_count=3, cashbook_count=3.
- **Request 1:** POST /rest/v1/rpc/create_payment, payment_method_id=2, amount=1000, reference_no=REG-SALE-13-DUP-001, notes=REG-SALE-13-FIRST-REQUEST. HTTP 200, payment id=15. SQL: paid=73000, due=37000, payment_count=4, payment_sum=73000, cashbook_count=4, cashbook_in_sum=73000.
- **Request 2:** Gửi lại nguyên vẹn payload. HTTP 200, tạo payment id=16. SQL: paid=74000, due=36000, payment_count=5, payment_sum=74000, cashbook_count=5, cashbook_in_sum=74000.
- **Expected:** Request thứ hai được nhận diện là retry, không tạo thêm payment hoặc phiếu thu. **Actual:** Ghi nhận hai payment và hai cashbook, tăng thu thêm 1000.

### DEF-SALE-013 — Tạo thanh toán trùng khi gửi lại request API

- **Trạng thái:** OPEN. **Severity:** Major. **Priority:** High. **Test case:** REG-SALE-13 FAIL.
- **Bằng chứng nguyên nhân:** create_payment có FOR UPDATE và kiểm tra công nợ nhưng không kiểm tra định danh yêu cầu; bảng payments không có idempotency key hoặc unique index tương ứng. Cơ chế hiện tại chặn vượt công nợ, không chặn retry giống nhau khi vẫn đủ công nợ.
- **Tiêu chí sửa:** Mỗi lần thanh toán logic có idempotency key ổn định, retry giữ nguyên key, thanh toán độc lập dùng key mới; đảm bảo nguyên tử tối đa một payment và một cashbook trên mỗi key, xử lý rõ trường hợp cùng key nhưng payload khác. Không chặn theo invoice+amount đơn thuần.
- **Retest/regression:** Chưa thực hiện. Giữ payment id 15 và 16 làm bằng chứng, không xóa trực tiếp dữ liệu tài chính.


## Bổ sung kết quả sửa lỗi DEF-SALE-013 và kiểm thử lại (14/10/2026)

**Giữ nguyên REG-SALE-13 FAIL và bằng chứng lỗi ban đầu phía trên.** Các ghi chú “chưa triển khai” ở phần lịch sử đã được thay thế bởi kết quả bên dưới. Severity: Major; Priority: High. **Trạng thái hiện tại: FIXED / chờ xác nhận đóng lỗi (OPEN)**; bước Postman kiểm tra RPC cũ được người kiểm thử chủ động bỏ qua, không được ghi PASS.

### Migration, RPC và frontend

- **MIG-PAY-013-01 PASS:** Bổ sung `payments.idempotency_key uuid`, `payments.request_fingerprint text`, unique partial index `ux_payments_idempotency_key`.
- **RPC `create_payment_v2`:** Yêu cầu UUID idempotency; kiểm tra đăng nhập và PAYMENT_CREATE; cùng key và cùng payload trả lại Payment đã có; cùng key nhưng payload khác trả HTTP 409 / 23505 `IDEMPOTENCY_KEY_CONFLICT`; kiểm tra công nợ, phương thức, ca và quỹ trước khi tạo Payment/Cashbook.
- **Frontend:** Cả Bán hàng và Thu nợ chuyển sang `create_payment_v2`; lưu UUID/pending state phía client. Commit `054cfba02e11ed976b3051c38a5d361479b9d35e` (tích hợp RPC v2), `d09f6428a4b7d561c38bbb0ead338ab9ff754b69` (bắt lỗi promise làm mới ca ở login). Smoke UI/login/Console PASS. Chưa chứng minh khả năng phục hồi mất phản hồi mạng và chống tạo trùng hóa đơn ban đầu.

### Test case backend — bằng chứng API và SQL

| Test Case | Kết quả thực tế | Status |
| --- | --- | --- |
| RETEST-SALE-013-01 | Thanh toán v2 lần đầu 1000 cho hóa đơn 8, UUID `f4d2a0c1-6e8b-4a79-9c25-7b1e3d6f9012`, Payment id 17. SQL paid=75000, due=35000, payment_count=6, cashbook_count=6 | PASS |
| RETEST-SALE-013-02 | Retry cùng key/payload trả lại id 17; SQL không đổi | PASS |
| RETEST-SALE-013-03 | Cùng key, đổi amount=2000 → HTTP 409, code 23505 `IDEMPOTENCY_KEY_CONFLICT`; SQL không đổi | PASS |
| REG-SALE-013-04 | Hai giao dịch độc lập, key khác nhau, 1000 mỗi giao dịch → Payment id 18,19; SQL paid=77000, due=33000, payment_count=8, cashbook_count=8 | PASS |
| REG-SALE-013-05 | EMPLOYEE gọi v2 → HTTP 403, code 42501, `FORBIDDEN: missing permission PAYMENT_CREATE`; SQL paid=77000, due=33000, payment_count=8, cashbook_count=8; unauthorized key count=0 | PASS |

### Test case frontend — bằng chứng UI và SQL

| Test Case | Thao tác / Actual | SQL Evidence | Status |
| --- | --- | --- | --- |
| REG-SALE-013-06-A | ADMIN thanh toán bán hàng HD00000013; không lỗi Console | Invoice id 13: total=50000, paid=50000, due=0, PAID; payment_count=1, cashbook_count=1, idempotent_payment_count=1 | PASS |
| REG-SALE-013-06-B | Nhấn thanh toán nhanh hai lần; báo `Đang xử lý hóa đơn, vui lòng chờ.`; chỉ ghi nhận một lần. **HD00000014 là bài kiểm thử dịch vụ 0đ riêng biệt, không phải bằng chứng lỗi của ca double-click.** Fixture đúng: HD00000015 | Invoice id 15: total=50000, paid=50000, due=0, PAID; payment_count=1, cashbook_count=1, idempotent_payment_count=1 | PASS |
| REG-SALE-013-06-C | Thu nợ 1000 CASH HD00000008 một lần, UI báo thành công, không lỗi Console; đã trả 77000→78000, còn nợ 33000→32000 | total=110000, paid=78000, due=32000, PARTIALLY_PAID, payment_count=9, idempotent_payment_count=4, latest_payment_id=22, latest_payment_amount=1000, cashbook_count=9 | PASS |
| REG-SALE-013-06-D | Nhấn Thu nợ 1000 CASH nhanh hai lần, chỉ một thông báo `Đã thu nợ 1.000 ₫ và đồng bộ Supabase.`, không lỗi Console; đã trả 78000→79000, nợ 32000→31000 | paid=79000, due=31000, PARTIALLY_PAID, payment_count=10, idempotent_payment_count=5, latest_payment_id=23, latest_payment_amount=1000, cashbook_count=10 | PASS |

**Lưu ý đối soát:** Các mốc SQL của hóa đơn 8 là dữ liệu cộng dồn theo thời gian. REG-SALE-013-06-D tăng đúng 1 Payment và 1 Cashbook so với mốc trước, không phải tạo 10 giao dịch mới.

### REG-SALE-013-07 — Thu hồi quyền RPC cũ

- **Trước sửa, SQL:** `create_payment` SECURITY DEFINER, quyền EXECUTE: anon=true, authenticated=true, service_role=true; `create_payment_v2`: anon=false, authenticated=true, service_role=true. RPC cũ còn đường gọi trực tiếp, không yêu cầu idempotency key.
- **MIG-PAY-013-03 thực thi thành công:** `REVOKE ALL ON FUNCTION public.create_payment(bigint,bigint,numeric,text,text) FROM PUBLIC, anon, authenticated;`
- **REG-SALE-013-07-A PASS (SQL):** `create_payment`: anon=false, authenticated=false, service_role=true; `create_payment_v2`: anon=false, authenticated=true, service_role=true.
- **REG-SALE-013-07-B NOT RUN / SKIPPED BY USER:** Không thực hiện Postman gọi `create_payment` bằng JWT ADMIN; **không có HTTP 403 thực tế** để ghi nhận. Kết quả SQL xác minh phân quyền là PASS nhưng kiểm thử API trực tiếp chưa thực hiện.
- **Phạm vi còn lại:** Cân nhắc chấp nhận bằng chứng SQL để đóng lỗi hoặc chạy bổ sung negative API test; chưa kiểm thử mất phản hồi/timeout và retry từ frontend; rà soát tích hợp khác dùng RPC cũ. Giữ nguyên Payment id 15,16 làm bằng chứng lỗi lịch sử; không xóa dữ liệu tài chính trực tiếp.

**Trạng thái DEF-SALE-013: OPEN / FIXED PENDING FINAL ACCEPTANCE.** Đã có bằng chứng PASS cho backend idempotency, UI Bán hàng/Thu nợ và quyền RPC cũ bằng SQL, nhưng chưa xác nhận đóng lỗi.

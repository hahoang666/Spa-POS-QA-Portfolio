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

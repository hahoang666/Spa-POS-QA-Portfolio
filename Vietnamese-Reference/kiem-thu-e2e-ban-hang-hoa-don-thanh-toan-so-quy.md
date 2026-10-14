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
| SALE-E2E-02 | SALE-02, SALE-04, SALE-05 | Functional / Integration / E2E | Thanh toán đủ bằng chuyển khoản, ghi đúng quỹ ngân hàng | CHƯA CHẠY; dự kiến bán 1 × DV000001, thanh toán BANK_TRANSFER 110.000 ₫, đối soát và reload | PAID, nợ 0; payment BANK_TRANSFER và giao dịch thu IN tương ứng trong quỹ ngân hàng; không ghi nhầm quỹ tiền mặt | High | Chưa thực hiện | NOT RUN | — | — | — |

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

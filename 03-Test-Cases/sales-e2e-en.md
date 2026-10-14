# Sales → Invoice → Payment → Cashbook — E2E Test Cases (English)

**Application:** [Spa POS](https://hahoang666.github.io/Spa-pos-pp/)  
**Requirements:** [REQUIREMENT_BASELINE_V1.0](https://github.com/hahoang666/Spa-pos-pp/blob/main/docs/REQUIREMENT_BASELINE_V1.0.md) (SALE-02, SALE-04, SALE-05, SALE-06)  
**Execution date:** 2026-10-14  
**Build / commit:** Not captured  
**Evidence:** Tester-provided UI observations and Supabase SQL query outputs in the QA session; screenshots not yet committed.

## Test case register

| Test Case ID | Requirement ID | Test Type | Scenario | Steps / Test Data | Expected Result | Priority | Actual Result | Status | Defect ID | Evidence | Build / Date |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| SALE-E2E-01 | SALE-02, SALE-04, SALE-05, SALE-06 | Functional / Integration / E2E | Full cash payment creates a paid invoice and matching cashbook receipt; data persists without duplicates | Open cashier shift; sell 1 × DV000001 with no discount, VAT 10%; pay 110,000 VND by CASH; reload; inspect invoices, invoice_items, payments, cashbook_transactions; count linked records | Invoice COMPLETED/PAID; subtotal 100,000, VAT 10,000, total/paid 110,000, due 0; one matching item, one CASH payment, one CASH fund IN receipt; persisted after reload; no duplicate linked records | Critical | Invoice HD00000010 (id 10) COMPLETED/PAID, subtotal 100000, VAT 10000, total/paid 110000, due 0. Item id 10: DV000001, qty 1, line total 110000. Payment id 9: CASH 110000, shift 4. Cashbook id 10: fund 1, IN 110000, payment_id 9, reference INVOICE/10. UI invoice and cashbook persisted after hard reload. Counts: items=1, payments=1, cashbook=1. | PASS | — | Tester-reported UI and SQL results; screenshot not committed | 2026-10-14; build not captured |
| SALE-E2E-02 | SALE-02, SALE-04, SALE-05 | Functional / Integration / E2E | Full bank-transfer payment records receipt in bank fund | Open shift 4; sell 1 × DV000001, VAT 10%, no discount; pay 110000 via BANK_TRANSFER; query invoices, invoice_items, payments, cashbook_transactions, funds; hard reload and check linked record counts | PAID, due 0; one BANK_TRANSFER payment and matching BANK_TRANSFER fund IN receipt; no CASH receipt; persisted, no duplicate records | High | HD00000011 (invoice id 11): COMPLETED/PAID, subtotal 100000, VAT 10000, total/paid 110000, due 0. Item id 11: DV000001, qty 1, total 110000. Payment id 10: BANK_TRANSFER 110000, shift 4. Cashbook id 11: fund 2 BANK_TRANSFER, IN 110000, payment_id 10, INVOICE/11. Fund reconciliation: BANK_TRANSFER count 1, total 110000; no CASH row. After reload invoice and bank receipt still visible. Counts items=1, payments=1, cashbook=1. | PASS | — | Tester-reported UI and five SQL query outputs plus post-reload duplicate count; screenshot not committed | 2026-10-14; build not captured |
| SALE-E2E-03 | SALE-02, SALE-04, SALE-05 | Functional / Integration / E2E | Partial CASH payment leaves outstanding debt and records only collected funds | Shift 4; sell 1 × DV000001, VAT 10%, no discount; collect CASH 70000 of 110000; verify invoice, item, payment and cashbook; hard reload; count linked records | Invoice COMPLETED/PARTIALLY_PAID, total 110000, paid 70000, due 40000; one CASH payment and CASH fund IN receipt for 70000 only; persisted, no duplicates | High | HD00000012 (invoice id 12): COMPLETED/PARTIALLY_PAID, subtotal 100000, VAT 10000, total 110000, paid 70000, due 40000. Item id 12: DV000001 × 1, total 110000. Payment id 11: CASH 70000, shift 4. Cashbook id 12: fund 1 CASH, IN 70000, payment_id 11, INVOICE/12. Invoice and cash receipt persisted after hard reload. Counts: items=1, payments=1, cashbook=1. | PASS | — | Tester-reported UI, four SQL query results and post-reload duplicate count; screenshots not committed | 2026-10-14; build not captured |
| SALE-E2E-04 | SALE-04, SALE-05, SALE-06 | Functional / Integration / E2E | Collect remaining debt with bank transfer | On HD00000012 (paid CASH 70000, due 40000), choose debt collection BANK_TRANSFER 40000 and confirm; reload; verify invoices/payments/cashbook | Invoice PAID, paid 110000, due 0; second BANK_TRANSFER payment 40000; second BANK_TRANSFER fund IN receipt 40000; persisted | High | Retest 2026-10-14: UI confirmed debt collection and Supabase sync; after reload invoice PAID with due 0, debt button hidden, bank fund increased 40000. SQL: invoice id 12 total/paid 110000, due 0; payments id 11 CASH 70000 and id 12 BANK_TRANSFER 40000; cashbook id 12 CASH IN 70000 and id 13 BANK_TRANSFER IN 40000, both linked to invoice 12. Original FAIL retained in defect history below. | PASS (RETEST) | DEF-SALE-004 | UI reload and three Supabase SQL result sets; merge commit a3865a1 | 2026-10-14; build not captured |

## SALE-E2E-01 — SQL evidence and reconciliation

| Source | Key | Observed result |
| --- | --- | --- |
| `public.invoices` | `id=10`, `code=HD00000010` | `status=COMPLETED`, `payment_status=PAID`, `subtotal_before_vat=100000`, `vat_amount=10000`, `discount_amount=0`, `total_amount=110000`, `paid_amount=110000`, `due_amount=0` |
| `public.invoice_items` | `id=10`, `invoice_id=10` | `product_code=DV000001`, `quantity=1`, `unit_price_before_vat=100000`, `unit_price_after_vat=110000`, `line_subtotal_before_vat=100000`, `line_vat_amount=10000`, `line_total_amount=110000` |
| `public.payments` | `id=9`, `invoice_id=10` | `payment_method_code=CASH`, `amount=110000`, `shift_id=4` |
| `public.cashbook_transactions` | `id=10`, `payment_id=9` | `fund_id=1`, `direction=IN`, `transaction_type=SALE`, `amount=110000`, `reference_type=INVOICE`, `reference_id=10`, `shift_id=4` |
| Persistence / duplicate check | After Ctrl+Shift+R | Invoice and cashbook entry still visible; SQL counts: `item_count=1`, `payment_count=1`, `cashbook_count=1` |

**Verdict:** PASS for the tested single-item, full-CASH payment scenario only. Other payment methods, failure paths, concurrency and broader duplicate-prevention behavior remain untested.

**SQL test script notes:** Initial verification queries referred to nonexistent columns `invoice_items.subtotal_before_vat` and `cashbook_transactions.type` (PostgreSQL 42703). These were test-query mistakes, not application defects. Corrected queries using the actual schema returned the results above.

## SALE-E2E-02 — SQL evidence and reconciliation

| Source | Key | Observed result |
| --- | --- | --- |
| `public.invoices` | `id=11`, `code=HD00000011` | `status=COMPLETED`, `payment_status=PAID`, `subtotal_before_vat=100000`, `vat_amount=10000`, `discount_amount=0`, `total_amount=110000`, `paid_amount=110000`, `due_amount=0` |
| `public.invoice_items` | `id=11`, `invoice_id=11` | `product_code=DV000001`, `quantity=1`, `unit_price_before_vat=100000`, `unit_price_after_vat=110000`, `vat_rate=10`, `line_subtotal_before_vat=100000`, `line_discount_amount=0`, `line_vat_amount=10000`, `line_total_amount=110000` |
| `public.payments` | `id=10`, `invoice_id=11` | `payment_method_id=2`, `payment_method_code=BANK_TRANSFER`, `amount=110000`, `shift_id=4` |
| `public.cashbook_transactions` | `id=11`, `payment_id=10` | `fund_id=2`, `fund_code=BANK_TRANSFER`, `shift_id=4`, `transaction_type=SALE`, `direction=IN`, `amount=110000`, `reference_type=INVOICE`, `reference_id=11` |
| Fund reconciliation | `HD00000011` | One `BANK_TRANSFER` row: `transaction_count=1`, `total_amount=110000`; no `CASH` row |
| Persistence / duplicate check | After Ctrl+Shift+R | Invoice and bank receipt still visible; SQL counts: `item_count=1`, `payment_count=1`, `cashbook_count=1` |

**Verdict:** PASS for the tested single-item full-bank-transfer scenario. Other partial/mixed payment and failure scenarios remain untested.

## SALE-E2E-03 — Partial cash payment SQL evidence

| Source | Key | Observed result |
| --- | --- | --- |
| `public.invoices` | `id=12`, `code=HD00000012` | `status=COMPLETED`, `payment_status=PARTIALLY_PAID`, `subtotal_before_vat=100000`, `vat_amount=10000`, `discount_amount=0`, `total_amount=110000`, `paid_amount=70000`, `due_amount=40000` |
| `public.invoice_items` | `id=12`, `invoice_id=12` | `product_code=DV000001`, `quantity=1`, `unit_price_before_vat=100000`, `unit_price_after_vat=110000`, `vat_rate=10`, `line_total_amount=110000` |
| `public.payments` | `id=11`, `invoice_id=12` | `payment_method_id=1`, `payment_method_code=CASH`, `amount=70000`, `shift_id=4` |
| `public.cashbook_transactions` | `id=12`, `payment_id=11` | `fund_id=1`, `fund_code=CASH`, `transaction_type=SALE`, `direction=IN`, `amount=70000`, `reference_type=INVOICE`, `reference_id=12`, `shift_id=4` |
| Persistence / duplicate | After Ctrl+Shift+R | Invoice still shows 70000 paid and 40000 due; cash receipt 70000 still visible; `item_count=1`, `payment_count=1`, `cashbook_count=1` |

**Verdict:** PASS for this partial CASH payment scenario. Later collection of the remaining 40000 and other payment/error paths are outside this test's verified scope.


## SALE-E2E-04 — Defect history and retest (2026-10-14)

- **Original FAIL / DEF-SALE-004:** UI falsely reported success; after reload invoice 12 stayed PARTIALLY_PAID with paid 70000, due 40000. Only payment 11 and cashbook 12 (CASH 70000) existed. Root cause: local-only `collectDebt` implementation.
- **Fix deployed:** PR #2 merged into `main`, squash commit `a3865a175ab25b1f9462ddf66877192d35f397f2`; frontend calls Supabase `create_payment` and refreshes backend sales data.
- **Retest PASS:** Invoice 12 `COMPLETED/PAID`, total 110000, paid 110000, due 0; payment 11 CASH 70000 + payment 12 BANK_TRANSFER 40000; cashbook 12 CASH IN 70000 + cashbook 13 BANK_TRANSFER IN 40000, both referencing invoice 12. UI persisted after reload; debt action hidden; bank cashbook +40000.
- **Defect disposition:** DEF-SALE-004 VERIFIED / CLOSED for reported scenario. Other regression and error-path cases remain untested.


## REG-SALE-05 — Reject collection exceeding remaining debt (2026-10-14)

- **Result:** PASS (UI regression and data integrity); direct API enforcement not yet tested.
- **Fixture:** HD00000008 (invoice id 8), PARTIALLY_PAID, paid 70000, due 40000.
- **Action:** Entered 40001, chose BANK_TRANSFER, confirmed once. UI displayed “Số tiền thu không hợp lệ.”; remaining debt stayed 40000.
- **SQL baseline and post-test:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — unchanged.
- **Console observation:** `index.ts.js:5 {error: "SOURCE_LANG_VI"}`; source not yet established, not attributed to the app.
- **Scope limitation:** Backend-side overpayment rejection via direct RPC/API remains unverified.


## REG-SALE-06 — Reject zero and negative debt collection (2026-10-14)

- **Result:** PASS (UI validation and SQL reconciliation); direct API enforcement not yet tested.
- **Invoice:** HD00000008, invoice id 8, outstanding debt 40000.
- **Actions:** Entered `0` and `-1000` as collection amounts and clicked Collect Debt separately. Both attempts displayed “Số tiền thu không hợp lệ.”; outstanding debt remained 40000.
- **Post-test SQL matched baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — unchanged.
- **Limitation:** Direct RPC `create_payment` validation for invalid amounts remains untested.


## REG-SALE-07 — Backend rejects debt overpayment (2026-10-14)

- **Result:** PASS — Postman RPC rejection and SQL reconciliation.
- **Fixture:** HD00000008, invoice id 8, due 40000.
- **Request:** `POST /rest/v1/rpc/create_payment` with `p_invoice_id=8`, `p_payment_method_id=2`, `p_amount=40001`, `p_reference_no=null`, `p_notes=REG-SALE-07-OVERPAYMENT`.
- **Response:** HTTP `400 Bad Request`, PostgreSQL code `22023`, message `Payment amount exceeds invoice due amount`.
- **Post-request SQL matched baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — unchanged.
- **Verdict:** Backend rejected overpayment without new financial records in this scenario. Zero/negative API inputs and other error paths remain untested.


## REG-SALE-08 — Backend rejects zero payment (2026-10-14)

- **Result:** PASS — Postman RPC rejection and SQL reconciliation.
- **Fixture:** HD00000008, invoice id 8, due 40000.
- **Request:** `POST /rest/v1/rpc/create_payment` with `p_invoice_id=8`, `p_payment_method_id=2`, `p_amount=0`, `p_notes=REG-SALE-08-ZERO-AMOUNT`.
- **Response:** HTTP `400 Bad Request`, code `22023`, message `Payment amount must be greater than 0`.
- **Post-request SQL matched baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — unchanged.
- **Verdict:** Backend rejected zero payment without new financial records. Negative payment API case remains untested.


## REG-SALE-09 — Backend rejects negative payment (2026-10-14)

- **Result:** PASS — Postman RPC rejection and SQL reconciliation.
- **Fixture:** HD00000008, invoice id 8, due 40000.
- **Request:** `POST /rest/v1/rpc/create_payment`, `p_invoice_id=8`, `p_payment_method_id=2`, `p_amount=-1000`, `p_notes=REG-SALE-09-NEGATIVE-AMOUNT`.
- **Response:** HTTP `400 Bad Request`, code `22023`, message `Payment amount must be greater than 0`.
- **Post-request SQL matched baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — unchanged.
- **Regression summary:** REG-SALE-05 through REG-SALE-09: **5/5 PASS** within tested scope.
- **Limitations:** Concurrency, system failure handling, and all permission/error paths not yet tested.


## REG-SALE-10 — create_payment API authorization (2026-10-14)

- **Result:** PASS — permission enforcement and SQL reconciliation.
- **Permission matrix:** `PAYMENT_CREATE`: ADMIN = GRANTED; CASHIER = NOT_GRANTED; EMPLOYEE = NOT_GRANTED (SQL check of `roles`, `permissions`, `role_permissions`).
- **Actor:** `employee01` (EMPLOYEE); `POST /rest/v1/rpc/create_payment` with `p_invoice_id=8`, `p_payment_method_id=2`, `p_amount=1000`, `p_notes=REG-SALE-10-UNAUTHORIZED`.
- **Response:** HTTP `403 Forbidden`, PostgreSQL code `42501`, message `FORBIDDEN: missing permission PAYMENT_CREATE`.
- **Post-request SQL matched baseline:** `payment_status=PARTIALLY_PAID`, `paid_amount=70000`, `due_amount=40000`, `payment_count=1`, `payment_sum=70000`, `cashbook_count=1`, `cashbook_in_sum=70000` — unchanged.
- **Verdict:** EMPLOYEE cannot create unauthorized payments; no financial data changes. **Review CASHIER requirements** because CASHIER also lacks `PAYMENT_CREATE`; not classified as a defect until checked against requirements.


## REG-SALE-11 — CASHIER payment authorization (2026-10-14)

- **Result:** FAIL against cashier payment business requirement; rejected transaction data integrity PASS.
- **Preconditions:** CASHIER has only ATTENDANCE_SELF, RETURN_CREATE, RETURN_VIEW, shift.open, shift.close, shift.view. INVOICE_CREATE, INVOICE_VIEW, PAYMENT_CREATE exist but are not granted to CASHIER.
- **Test:** Authenticated CASHIER calls `POST /rest/v1/rpc/create_payment` for invoice 8, payment method 2, amount 1000, note REG-SALE-11-CASHIER-AUTH.
- **Actual:** HTTP 403, code 42501, `FORBIDDEN: missing permission PAYMENT_CREATE`.
- **SQL after rejected call:** Invoice HD00000008 remains PARTIALLY_PAID, paid=70000, due=40000; payment_count=1, payment_sum=70000; cashbook_count=1, cashbook_in_sum=70000 (same as baseline).
- **Defect:** DEF-SALE-011, OPEN; suspected role-permission configuration mismatch, Severity Major, Priority High (proposed). Security denial behaves correctly per current grants; cashier business flow is blocked. Confirm exact role mapping and payment-vs-debt scope before fixing. No permissions changed.

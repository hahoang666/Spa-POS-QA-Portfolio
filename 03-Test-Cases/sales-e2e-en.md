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
| SALE-E2E-02 | SALE-02, SALE-04, SALE-05 | Functional / Integration / E2E | Full bank-transfer payment records receipt in bank fund | NOT RUN; proposed: sell 1 × DV000001, pay 110,000 VND via BANK_TRANSFER, verify invoice/payment/cashbook and reload | PAID, due 0; BANK_TRANSFER payment and matching bank fund IN transaction; no cash fund receipt | High | Not executed | NOT RUN | — | — | — |

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

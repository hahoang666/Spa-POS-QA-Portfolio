# DEF-SALE-013 — Idempotency Retest and Regression (2026-10-14)

**Original defect:** REG-SALE-13 FAIL; identical legacy create_payment retry generated duplicate payments 15 and 16 and corresponding cashbook receipts. Severity Major, Priority High. Preserve original FAIL. **Current defect status: OPEN / FIXED PENDING FINAL ACCEPTANCE.**

## Implementation and evidence
- MIG-PAY-013-01 PASS: payments.idempotency_key UUID, request_fingerprint text, partial unique index ux_payments_idempotency_key.
- create_payment_v2: same key/payload returns original payment; same key/changed payload returns HTTP 409, SQLSTATE 23505; permission PAYMENT_CREATE required.
- Frontend cashier checkout and debt collection migrated to create_payment_v2; commits 054cfba02e11ed976b3051c38a5d361479b9d35e and d09f6428a4b7d561c38bbb0ead338ab9ff754b69.
- MIG-PAY-013-03 PASS: REVOKE ALL ON FUNCTION public.create_payment(bigint,bigint,numeric,text,text) FROM PUBLIC, anon, authenticated.

| Test | Observed evidence | Result |
| --- | --- | --- |
| RETEST-SALE-013-01 | First v2 payment id 17, amount 1000, invoice 8; SQL paid 75000, due 35000, payments 6, cashbook 6 | PASS |
| RETEST-SALE-013-02 | Retry same UUID and payload returned payment id 17; SQL unchanged | PASS |
| RETEST-SALE-013-03 | Same UUID, changed amount 2000: HTTP 409 / 23505 IDEMPOTENCY_KEY_CONFLICT; SQL unchanged | PASS |
| REG-SALE-013-04 | Two independent UUIDs: payment ids 18,19, amount 1000 each; SQL paid 77000, due 33000, payments 8, cashbook 8 | PASS |
| REG-SALE-013-05 | EMPLOYEE unauthorized: HTTP 403 / 42501; SQL paid 77000, due 33000, payments 8, cashbook 8, unauthorized key count 0 | PASS |
| REG-SALE-013-06-A | ADMIN checkout HD00000013; SQL total/paid 50000, due 0, PAID, payments 1, cashbook 1, idempotent payments 1; no Console error | PASS |
| REG-SALE-013-06-B | Double-click checkout: one completion, in-progress alert; fixture HD00000015 total/paid 50000, due 0, PAID, payments 1, cashbook 1, idempotent payments 1 | PASS |
| REG-SALE-013-06-C | HD00000008 debt CASH 1000: UI success; SQL paid 78000, due 32000, PARTIALLY_PAID, payments 9, idempotent payments 4, latest payment id 22 amount 1000, cashbook 9 | PASS |
| REG-SALE-013-06-D | Rapid double-click debt CASH 1000: one success message, no console error; SQL paid 79000, due 31000, PARTIALLY_PAID, payments 10, idempotent payments 5, latest payment id 23 amount 1000, cashbook 10 | PASS |
| REG-SALE-013-07-A | SQL privilege check after REVOKE: legacy create_payment anon=false authenticated=false service_role=true; v2 anon=false authenticated=true service_role=true | PASS |
| REG-SALE-013-07-B | Direct Postman call to legacy RPC using ADMIN JWT intentionally skipped by tester; no HTTP status observed | NOT RUN / SKIPPED |

**Clarifications:** HD00000014 was a separate intentional zero-price service test, not the double-click payment fixture. Invoice 8 SQL counts are cumulative; REG-SALE-013-06-D adds only one payment and cashbook relative to 06-C. Original duplicate payment records 15/16 remain historical evidence.

**Residual coverage gaps:** direct legacy-RPC Postman denial not tested; frontend lost-response/timeout retry and invoice creation idempotency not verified. Do not mark these PASS or close DEF-SALE-013 solely from the observed cases.

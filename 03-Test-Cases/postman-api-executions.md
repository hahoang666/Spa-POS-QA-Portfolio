# Postman / Supabase API — Historical Test Cases

**Execution:** 8–9 October 2026 (historical tester-reported observations). **Build:** Not recorded. **Evidence:** Raw sanitized Postman screenshots/exports not attached; marked Chờ bổ sung. Requirement IDs use known permission/baseline identifiers where applicable.

## Test case register

| Test Case ID | Requirement ID | Test Type | Scenario | Steps / Test Data | Expected Result | Priority | Actual Result | Status | Defect ID | Evidence | Build / Date |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC-SHIFT-01 | SHIFT_OPEN | API / Functional | Open shift with valid cash | Authenticated open_shift request with valid opening cash | Shift opens successfully | High | Shift opened successfully | PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-SHIFT-02 | SHIFT_OPEN | API / Negative | Duplicate open shift | Call open_shift while same account already has an open shift | Reject duplicate shift | High | SHIFT_ALREADY_OPEN returned | PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-SHIFT-03 | SHIFT_CLOSE | API / Functional | Close shift with variance | Close shift with actual cash 900000 against expected 1000000 | Shift closes; variance -100000 recorded | High | Variance -100000 reported | PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-SHIFT-04 | SHIFT_OPEN | API / Validation | Invalid opening cash | Submit invalid opening cash to open_shift | Invalid amount rejected | Medium | API rejected request; exact payload/status not recorded | PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-SHIFT-05 | SHIFT_CLOSE | API / Authorization | Close another user's shift | Authenticated User B attempts to close User A's open shift | FORBIDDEN; shift unchanged | Critical | FORBIDDEN response observed; post-request shift state not fully evidenced | PASS (reported) | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-PAYMENT-01 | SALE-04, SALE-05 | API / Functional | Full invoice payment | Pay 110000 on invoice due 110000 using authorized payment RPC | Invoice becomes PAID; due 0 | High | Invoice changed to PAID | PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-PAYMENT-02 | SALE-04, SALE-05 | API / Functional | Partial payment | Pay 60000 on invoice total/due 110000 | Invoice PARTIALLY_PAID; due 50000 | High | PARTIALLY_PAID reported | PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-PAYMENT-03 | SALE-04, SALE-05 | API / Functional | Pay remaining balance | Pay remaining 50000 on partially paid 110000 invoice | Invoice becomes PAID; due 0 | High | Invoice changed to PAID | PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-PAYMENT-04 | SALE-04 | API / Negative | Reject overpayment | Submit payment greater than outstanding invoice due | Reject payment; no overcollection | Critical | Request rejected; exact response not captured in this historical record | PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-PAYMENT-05 | SALE-04 | API / Negative | Reject zero payment | Submit payment amount 0 | Reject invalid amount; no payment created | High | Request rejected; exact response not captured in this historical record | PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-PAYMENT-13 | INVOICE_CANCEL | API / Retest | Cancel nonexistent invoice | Call cancel_invoice with invoice id 999999 | HTTP 400 / 22023 / INVOICE_NOT_FOUND | High | Before fix HTTP 500 / P0002; after fix HTTP 400 / 22023 / INVOICE_NOT_FOUND | RETEST PASS | — | Chờ bổ sung | 2026-10-08–09; build not recorded |
| TC-PAYMENT-14 | INVOICE_CANCEL | API / Validation | Invalid invoice identifier | Submit invalid invoice ID to cancel_invoice; exact test value not recovered | Reject invalid ID without changing data | Medium | No confirmed execution result recovered | NOT VERIFIED | — | Chờ bổ sung | 2026-10-08–09; build not recorded |

## Retest reference — TC-PAYMENT-13

Original HTTP 500 / P0002 / INVOICE_NOT_FOUND; after migration, HTTP 400 / 22023 / INVOICE_NOT_FOUND. [Source fix](https://github.com/hahoang666/Spa-pos-pp/commit/5d72b2f3800720302f78bdd88b3dbfaab32ddf7b). The after-fix result is historical tester-reported, not a new execution.

## Evidence safety

Before uploading Postman proof, redact Authorization headers, access tokens, keys and personal data. Attach request method/endpoint, sanitized payload, HTTP status/response, and SQL checks where available.

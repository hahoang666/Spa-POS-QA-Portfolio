# API & SQL Test Execution — Reviewed Historical Evidence

**Project:** Spa POS | **Source:** user-supplied `Tổng hợp APT +SQL test.xlsx` | **Prepared:** 13 October 2026 | **Execution:** historical, not re-run.

## Test execution register

| Case ID | Scenario | Expected | Observed evidence | Assessment |
|---|---|---|---|---|
| TC-SHIFT-01 | Open shift with 1,000,000 opening cash | Shift OPEN | Shift row showed opening_cash=1000000 and status OPEN | Reported PASS |
| TC-SHIFT-02 | Open another shift while one is OPEN | Reject duplicate | HTTP 409, SQLSTATE 23505 | Reported PASS |
| TC-SHIFT-03 | Negative opening cash (-1) | Reject invalid input | HTTP 400, SQLSTATE 22023 | Reported PASS |
| TC-SHIFT-04 | Retrieve current shift | Return current shift | HTTP 200 recorded; response detail limited | Evidence incomplete |
| TC-SHIFT-05 | Close shift successfully | CLOSED | HTTP 200; SQL verification supplied | Reported PASS |
| TC-SHIFT-06 | Close an already CLOSED shift | 4xx, SHIFT_NOT_OPEN | **Before fix:** HTTP 500 / 55000. **After fix:** HTTP 409 / 23505 / SHIFT_NOT_OPEN (separate retest history) | Retest PASS reported |
| TC-SHIFT-07 | Close shift with 900,000 actual vs 1,000,000 expected | CLOSED, difference -100,000 | HTTP 200 and shift response | Reported PASS |
| TC-SHIFT-09 | Get current shift when none OPEN | No open shift | Source records a result but no complete response body | Evidence incomplete |
| TC-SHIFT-10 | User lacks shift.open | Authorization rejection | HTTP 403 reported | Reported PASS |
| TC-SHIFT-11 | User tries to close someone else's shift | FORBIDDEN; no DB change | HTTP 403 / 42501; SQL post-state actual **not provided** | API rule PASS; DB verification pending |
| TC-PAYMENT-01 | Cash payment and cashbook posting | Payment and cashbook entry | HTTP 200; source includes cashbook SQL notes | Reported PASS; reconcile row linkage |
| TC-PAYMENT-03 | Partial payment of 60,000 on 110,000 | PARTIALLY_PAID; due 50,000 | HTTP 200, payment response | Reported PASS |
| TC-PAYMENT-04 | Pay remaining 50,000 | PAID; due 0 | HTTP 200 and invoice SQL row | Reported PASS |
| TC-PAYMENT-05 | Pay more than outstanding | Reject; no new payment | HTTP 400 / 22023; SQL query/results recorded | Reported PASS |
| TC-PAYMENT-06 | Payment amount zero | Reject; no new payment | HTTP 400 / 22023; SQL query/results recorded | Reported PASS |
| TC-PAYMENT-07 | Negative payment amount | Reject | HTTP 400 / 22023 shown in source response; earlier draft had blank status | Reported PASS based on source response |
| TC-PAYMENT-08 | Pay an already PAID invoice | Reject; no new payment/cashbook | HTTP 400; post-request DB reconciliation not supplied | API rule PASS; DB verification pending |

**ID policy:** Preserve original case IDs (including gaps) for traceability. Do not renumber TC-PAYMENT-03 to 02 or TC-SHIFT-09 to 08.

## Supabase SQL verification

| Check | Query purpose | Evidence |
|---|---|---|
| Open shift | SELECT from public.shifts | Actual OPEN row recorded |
| Close shift | SELECT status, expected_cash, actual_cash, difference | Actual CLOSED row recorded |
| Unauthorized close | SELECT shift ownership/status after rejected request | **Actual result missing**; cannot confirm no mutation |
| Remaining payment | SELECT invoice 3 balances | Actual PAID and due 0 recorded |
| Overpayment rejection | SELECT payments and invoice balance | Actual result recorded |
| Zero payment rejection | SELECT payments and invoice balance | Actual result recorded |

Original SQL text and response details remain in the source workbook; avoid pasting unsanitized user IDs or credentials into this public repository.

## Defect & retest — DEF-SHIFT-001

**Title:** Closing a CLOSED shift returns server error instead of business conflict.

- **Expected:** HTTP 409 with `SHIFT_NOT_OPEN`.
- **Before fix:** HTTP 500 / SQLSTATE 55000 / `SHIFT_NOT_OPEN`.
- **After fix:** HTTP 409 / SQLSTATE 23505 / `SHIFT_NOT_OPEN` (reported in prior retest).
- **Retest:** PASS (historical report; screenshot and exact build SHA not attached).

## QA reporting rules

`Reported PASS` reflects the original tester's historical observation, not a newly executed test. `Evidence incomplete` means response, SQL result, or build metadata is missing. HTTP rejection does **not** independently prove that no database mutation occurred. This report is **not** a current-build regression sign-off.

**Safety:** do not publish JWTs, access tokens, private keys, passwords, real customer records, or raw authenticated request headers.

# Postman / Supabase API — Historical Executions

**Evidence origin:** user-reported Postman responses and Supabase checks in conversations from 8–9 October 2026. Raw Postman export, timestamps and sanitized screenshots still need to be attached.

| ID | Test | Actual historical result | Outcome |
|---|---|---|---|
| TC-SHIFT-01 | Open shift with valid opening cash | Opened successfully | PASS (reported) |
| TC-SHIFT-02 | Duplicate open shift | SHIFT_ALREADY_OPEN | PASS (reported) |
| TC-SHIFT-03 | Close with actual cash variance | Variance -100,000 for expected 1,000,000 / actual 900,000 | PASS (reported) |
| TC-SHIFT-04 | Invalid opening cash | Rejected | PASS (reported) |
| TC-SHIFT-05 | Close another user's shift | FORBIDDEN | PASS (reported) |
| TC-PAYMENT-01 | Pay invoice 110,000 in full | PAID | PASS (reported) |
| TC-PAYMENT-02 | Pay 60,000 of 110,000 | PARTIALLY_PAID | PASS (reported) |
| TC-PAYMENT-03 | Pay remaining 50,000 | PAID | PASS (reported) |
| TC-PAYMENT-04 | Overpay invoice | Rejected | PASS (reported) |
| TC-PAYMENT-05 | Pay zero amount | Rejected | PASS (reported) |
| TC-PAYMENT-13 | Cancel nonexistent invoice 999999 | Before: HTTP 500 / P0002 / INVOICE_NOT_FOUND. After fix: HTTP 400 / 22023 / INVOICE_NOT_FOUND | RETEST PASS |
| TC-PAYMENT-14 | Invalid invoice ID | No confirmed execution result recovered | NOT VERIFIED |

### TC-PAYMENT-13 — detailed retest

- **Precondition:** authenticated request to the `cancel_invoice` Supabase RPC.
- **Input:** nonexistent invoice ID `999999`.
- **Expected:** client error (HTTP 400), code `22023`, message `INVOICE_NOT_FOUND`.
- **Actual before fix:** HTTP 500, code `P0002`, message `INVOICE_NOT_FOUND`.
- **Fix:** migration in [source commit](https://github.com/hahoang666/Spa-pos-pp/commit/5d72b2f3800720302f78bdd88b3dbfaab32ddf7b).
- **Actual after fix:** HTTP 400, code `22023`, message `INVOICE_NOT_FOUND`.
- **Status:** PASS after retest, as reported in the conversation.

### API validation checklist
Check HTTP method and endpoint, authentication, payload, response status/body, permission and state rules, and read-only DB state after success or rejection. Never upload tokens or unredacted headers.

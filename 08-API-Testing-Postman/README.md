# Postman API Testing — Spa POS

## Executed historical coverage
- Shift RPC: open_shift, get_current_shift, close_shift.
- Payment RPC: create_payment, invoice status transitions and invalid payment amounts.
- Cancellation RPC: cancel_invoice, missing invoice and paid/unpaid business rules.

**Key retests:** [shift 409 response](../05-Retest-Evidence/api-retest-evidence.md), [invoice 400 response](../03-Test-Cases/postman-api-executions.md).

## How the test was validated
1. Send request with authenticated test account in Postman.
2. Compare HTTP status and JSON error or response.
3. Check permission and state transitions.
4. Verify database effects in Supabase SQL Editor where applicable.

## Planned artifacts (not yet uploaded)
Sanitized `.postman_collection.json`, sanitized environment variables, request/response screenshots and automated assertions. **Do not commit JWT, API secrets, refresh tokens or passwords.**

## Evidence
[Historical execution cases](../03-Test-Cases/postman-api-executions.md) and [API retests](../05-Retest-Evidence/api-retest-evidence.md).

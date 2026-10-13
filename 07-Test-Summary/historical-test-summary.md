# QA Test Summary — Historical Evidence

**Project:** Spa POS web demo (personal project with AI assistance)

## Verified from previous user-reported work
- Postman shift RPC tests and a closed-shift error mapping retest (409 / 23505 / SHIFT_NOT_OPEN).
- Postman missing-invoice cancellation retest (400 / 22023 / INVOICE_NOT_FOUND).
- Partial/full invoice payment state changes and cashbook reconciliation checks.
- Supabase SQL validation of schema, RLS, payment/return records and attendance record.
- Frontend regression for invoice cancellation and HR edit/account linking.

## Not yet included as proof
- Sanitized exported Postman collection and environment.
- Screenshots with request/response, execution timestamps and build SHA.
- Full current-build regression and final data integrity checks for every negative request.
- Complete attendance checkout E2E evidence.

## Conclusion
This portfolio demonstrates practical learning and documented QA execution, not commercial employment or a current production release certification. Historical statuses are intentionally retained with caveats.

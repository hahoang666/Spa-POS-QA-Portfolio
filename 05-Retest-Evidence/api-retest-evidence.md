# Historical Retest Evidence

## DEF-SHIFT-001
- **Before:** close_shift on a CLOSED shift returned HTTP 500 with SQLSTATE 55000.
- **After:** HTTP 409 Conflict; code 23505; message SHIFT_NOT_OPEN.
- **Evidence:** user-reported Postman response on 8 October 2026.
- **Result:** RETEST PASS.

## TC-PAYMENT-13 / missing invoice
- **Before:** invoice 999999 returned HTTP 500 / P0002 / INVOICE_NOT_FOUND.
- **After:** HTTP 400 / 22023 / INVOICE_NOT_FOUND.
- **Evidence:** Postman retest reported on 9 October 2026; [migration commit](https://github.com/hahoang666/Spa-pos-pp/commit/5d72b2f3800720302f78bdd88b3dbfaab32ddf7b).
- **Result:** RETEST PASS.

## Employee synchronization
- Four employee sync tests previously recorded FAIL; subsequent retest was reported 4/4 PASS. Keep both dated observations, not a stale OPEN status.

## Frontend regression
- HD00000004 canceled invoice displayed correctly after fix.
- EMP000002 edit and linked account display both passed HR-REGRESSION-01.

**Missing artifacts:** original Postman exports, screenshots, complete build metadata and linked issue tickets. No screenshots are fabricated here.

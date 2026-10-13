# Historical Defect Log

| Defect | Observation | Latest supported status |
|---|---|---|
| DEF-SHIFT-001 | Closing CLOSED shift originally returned HTTP 500 / 55000; expected business rejection | FIXED / RETEST PASS: 409, 23505, SHIFT_NOT_OPEN |
| DEF-PAYMENT-API-13 | Missing invoice cancellation returned 500 / P0002 instead of client error | FIXED / RETEST PASS: 400, 22023, INVOICE_NOT_FOUND |
| DEF-INVOICE-STATUS | HD00000004 backend CANCELLED while UI displayed outstanding debt | FIXED / regression PASS reported after refresh correction |
| DEF-PRODUCT-TYPE | PACKAGE insert failed constraint products_type_check (23514) | Historical failure; later PACKAGE products persisted; exact fix evidence to consolidate |
| DEF-HR-EDIT | Employee edit produced frontend Unexpected end of input | FIXED / HR-REGRESSION-01 PASS reported |
| DEF-HR-LINK | Linked employee account not visible in HR UI | FIXED / HR-REGRESSION-01 PASS reported |
| DEF-ATT-CHECKOUT | Attendance punch-out encountered JWT error | Resolution not independently confirmed in recovered evidence |
| DEF-EMP-SYNC-001..004 | Earlier employee sync/display mapping checks failed | **Superseded by later 4/4 PASS retest**; retain as historical defects, not currently OPEN without fresh reproduction |

Severity/priority and root cause should be assigned only when supported by reproducible evidence. This table is not an independently verified current open-bug list.

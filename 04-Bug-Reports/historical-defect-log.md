# Historical Defect Register — Spa POS

> **Historical snapshot, not a current open-defect list.** This register preserves reported defect outcomes; do not infer a closure date, owner, or retest evidence that has not been recorded.

| Defect ID | Summary | Module | Original Actual Result | Latest Reported Result | Status / Disposition | Severity | Priority | Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| DEF-SHIFT-001 | Shift close on CLOSED shift | Shift / API | HTTP 500 / 55000 on closed shift | 409 / 23505 / SHIFT_NOT_OPEN | FIXED / RETEST PASS | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-PAYMENT-API-13 | Cancel nonexistent invoice | Invoice / API | HTTP 500 / P0002 / INVOICE_NOT_FOUND | 400 / 22023 / INVOICE_NOT_FOUND | FIXED / RETEST PASS | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-INVOICE-STATUS | Canceled invoice displayed outstanding debt | Invoice / UI | HD00000004 backend CANCELLED but UI showed debt | UI refresh corrected; regression PASS reported | FIXED / REGRESSION PASS (reported) | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-PRODUCT-TYPE | PACKAGE product rejected | Products / Backend | products_type_check / 23514 on PACKAGE insert | Later PACKAGE records persisted; exact fix/retest artifact missing | HISTORICAL / VERIFY FIX | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-HR-EDIT | Employee edit syntax error | HR / Frontend | Unexpected end of input on edit | HR-REGRESSION-01 PASS reported | FIXED / REGRESSION PASS (reported) | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-HR-LINK | Linked account hidden in HR | HR / Frontend | Linked account not displayed | HR-REGRESSION-01 PASS reported | FIXED / REGRESSION PASS (reported) | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-ATT-CHECKOUT | Attendance punch-out JWT error | Attendance / API | JWT error on punch-out | No confirmed resolution recovered | STATUS UNCONFIRMED | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-EMP-SYNC-001 | Employee sync/display historical failure | HR / Data | Historical employee sync/display mapping failure | Later 4/4 PASS retest reported | SUPERSEDED / HISTORICAL | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-EMP-SYNC-002 | Employee sync/display historical failure | HR / Data | Historical employee sync/display mapping failure | Later 4/4 PASS retest reported | SUPERSEDED / HISTORICAL | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-EMP-SYNC-003 | Employee sync/display historical failure | HR / Data | Historical employee sync/display mapping failure | Later 4/4 PASS retest reported | SUPERSEDED / HISTORICAL | Chưa phân loại | Chưa phân loại | Chờ bổ sung |
| DEF-EMP-SYNC-004 | Employee sync/display historical failure | HR / Data | Historical employee sync/display mapping failure | Later 4/4 PASS retest reported | SUPERSEDED / HISTORICAL | Chưa phân loại | Chưa phân loại | Chờ bổ sung |

## Reporting rules

- **FIXED / RETEST PASS (reported)** means a successful historical retest was described; it does not imply independently verified attachment or formal closure approval.
- **HISTORICAL / VERIFY FIX** and **STATUS UNCONFIRMED** must not be represented as OPEN or CLOSED without fresh evidence.
- A single defect report should include: ID, summary, module, requirement, environment/build, preconditions, steps, expected/actual, impact, severity, priority, status, evidence, retest and owner where known.
- Do not invent issue IDs, root causes, assignees, timestamps or links. Add sanitized screenshots and SQL/API artifacts when available.

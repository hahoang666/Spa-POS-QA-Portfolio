# Historical Test Scenarios

| Area | Scenario | Historical observation |
|---|---|---|
| Shift API | Open shift with valid opening cash | Successful opening reported |
| Shift API | Open second shift while one is open | SHIFT_ALREADY_OPEN rejection reported |
| Shift API | Close an already closed shift | Error mapping fixed; retest 409 / 23505 / SHIFT_NOT_OPEN |
| Shift API | Close another user's shift | FORBIDDEN reported |
| Payment | Full payment | PAID reported |
| Payment | Partial payment and balance | PARTIALLY_PAID then PAID reported |
| Payment | Zero or excessive payment | Rejected |
| Invoice | Cancel unpaid invoice | CANCELLED; frontend stale status subsequently fixed |
| Invoice | Cancel paid invoice | Rejection reported; full data-integrity comparison remained pending in earlier report |
| Invoice | Cancel nonexistent invoice | Retest 400 / 22023 / INVOICE_NOT_FOUND |
| Refund | Reverse and refund return for HD00000009 | Cashbook OUT 110,000 and REFUNDED status observed |
| HR | Employee CRUD, persistence, linking and role checks | Later PASS observations; older sync FAIL entries are historical |
| Appointment | Date filter today/week/month | APPT-DATE-01..03 reported PASS |
| Cashbook | Card fund and totals | CARD-01..05 reported PASS |
| Attendance | Punch-in with GPS | DB record created; checkout and complete E2E not confirmed by same evidence |

These are scenario summaries, not a claim that every scenario was executed on the latest commit.

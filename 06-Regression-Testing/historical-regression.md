# Historical Regression Report

**Scope:** selected regression outcomes reported between 8–12 October 2026. **Current-build execution:** not performed.

| Area | Checks | Historical outcome |
|---|---|---|
| Shift API | open/current/close, invalid inputs, permissions | 9/9 shift API cases reported PASS after retest in earlier session; case-by-case raw export not attached |
| Invoice | HD00000004 cancellation frontend status and action visibility | PASS reported |
| HR | EMP000002 edit, linked account and console | HR-REGRESSION-01 PASS reported |
| Employee sync | 4 cases previously FAIL | Later 4/4 PASS reported |
| Appointment | APPT-DATE-01..03 | PASS reported |
| Cashbook | CARD-01..05 | PASS reported |
| Attendance | punch-in and punch-out | Punch-in record confirmed; complete E2E not established by recovered evidence |

### Release decision
Not a release sign-off. Retest coverage and evidence are historical and incomplete for the current application revision.

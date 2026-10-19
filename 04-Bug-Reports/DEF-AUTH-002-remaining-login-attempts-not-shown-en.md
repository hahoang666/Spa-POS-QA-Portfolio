# DEF-AUTH-002 — Remaining login attempts not displayed

## Defect details

| Field | Value |
| --- | --- |
| Defect ID | DEF-AUTH-002 |
| Project | Spa POS (web demo) |
| Module | Authentication / Login |
| Related requirement | AUTH-01 |
| Related test case | [AUTH-VAL-04](../03-Test-Cases/authentication-ui-validation-en.md) |
| Defect type | Functional / Validation feedback |
| Severity | Minor (proposed; pending triage) |
| Priority | Medium (proposed; pending triage) |
| Status | OPEN |
| Detected on | 2026-10-14 |
| Last reproduction | 2026-10-14 |
| Environment | Spa POS web; browser version and build SHA not recorded |
| Assigned to | Unassigned / not recorded |
| Retest status | NOT RUN — no confirmed fix |
| Evidence | [AUTH-VAL-04-invalid-password.png](../11-Test-Execution-Evidence/01-Authentication/AUTH-VAL-04-invalid-password.png) |

## Preconditions

Logged-out test session. Use an authorized disposable account for repeated failures.

## Steps to reproduce

1. Open the login page.
2. Enter test username `abc123` and an invalid test password.
3. Click Login and inspect the error and remaining-attempt indicator.

## Expected result

Login is rejected with a generic invalid-credentials message **and** an accurate remaining-attempt count per AUTH-01.

## Actual result

Login was rejected; message **“Tên đăng nhập hoặc mật khẩu không đúng.”** appeared with red password outline. No remaining-attempt count was shown.

## Impact / Risk

Required feedback is missing. This defect is separate from server-side lockout enforcement (DEF-AUTH-003).

## Suggested investigation / fix

Display a reliable remaining-attempt count consistent with backend lockout state, without revealing account existence.

## Retest / Exit criteria

After implementation, verify successive failures update the count correctly; coordinate with AUTH-VAL-07 lockout retest.

## Evidence and limitations

Screenshot shows the visible error state, not the full attempt history.


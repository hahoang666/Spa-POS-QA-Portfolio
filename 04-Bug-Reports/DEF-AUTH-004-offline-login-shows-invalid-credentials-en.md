# DEF-AUTH-004 — Offline login displays incorrect-credentials error

## Defect details

| Field | Value |
| --- | --- |
| Defect ID | DEF-AUTH-004 |
| Project | Spa POS (web demo) |
| Module | Authentication / Login |
| Related requirement | AUTH-01 |
| Related test case | [AUTH-VAL-15](../03-Test-Cases/authentication-ui-validation-en.md) |
| Defect type | Functional / Error handling |
| Severity | Minor (proposed; pending triage) |
| Priority | Medium (proposed; pending triage) |
| Status | OPEN |
| Detected on | 2026-10-14 |
| Last reproduction | 2026-10-14 |
| Environment | Spa POS web; Chrome DevTools Network set to Offline; browser version/build SHA not recorded |
| Assigned to | Unassigned / not recorded |
| Retest status | NOT RUN — no confirmed fix |
| Evidence | Chờ bổ sung |

## Preconditions

Logged-out session; valid authorized test credentials; DevTools Network throttling set to **Offline** with DevTools kept open.

## Steps to reproduce

1. Open the login screen and enable DevTools Network → Offline.
2. Enter valid test-account credentials.
3. Click Login and observe the error message and whether Dashboard is blocked.
4. Try Login again to check button recovery.

## Expected result

A connectivity-specific error is displayed, Dashboard access is denied, and the Login button becomes usable for retry.

## Actual result

Dashboard was blocked, but the UI displayed **“Tên tài khoản hoặc mật khẩu không đúng”** instead of a network error. Retry remained responsive.

## Impact / Risk

Misleading feedback may cause users to believe their credentials are wrong during a network outage.

## Suggested investigation / fix

Differentiate transport/network errors from invalid-credential responses; keep retry behavior.

## Retest / Exit criteria

After a fix, repeat with DevTools still Offline, verify connectivity message, no Dashboard access and retry; restore No throttling afterward.

## Evidence and limitations

A later login succeeded after DevTools was closed; Offline state was not reverified and this does not indicate authentication bypass.


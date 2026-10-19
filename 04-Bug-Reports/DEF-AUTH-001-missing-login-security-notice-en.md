# DEF-AUTH-001 — Required security notice missing on login screen

## Defect details

| Field | Value |
| --- | --- |
| Defect ID | DEF-AUTH-001 |
| Project | Spa POS (web demo) |
| Module | Authentication / Login |
| Related requirement | AUTH-05 |
| Related test case | [AUTH-UI-05](../03-Test-Cases/authentication-ui-validation-en.md) |
| Defect type | UI / Requirement compliance |
| Severity | Minor (proposed; pending triage) |
| Priority | Medium (proposed; pending triage) |
| Status | OPEN |
| Detected on | 2026-10-13 |
| Last reproduction | 2026-10-13 |
| Environment | Spa POS web; browser version and build SHA not recorded |
| Assigned to | Unassigned / not recorded |
| Retest status | NOT RUN — no confirmed fix |
| Evidence | Chờ bổ sung |

## Preconditions

User is logged out and the login page is displayed.

## Steps to reproduce

1. Open the Spa POS login page.
2. Inspect the entire login form for the notice specified in AUTH-05.

## Expected result

The login page displays the required notice: **“Không lưu thông tin đăng nhập trên thiết bị”**.

## Actual result

The required security notice is not displayed. Other login controls remain visible.

## Impact / Risk

Documented login-screen information requirement is unmet. This observation does **not** prove insecure credential storage.

## Suggested investigation / fix

Confirm the baseline wording is compatible with Remember Username and session restoration; implement the approved notice.

## Retest / Exit criteria

After a fix, verify the notice is visible on supported desktop viewport(s), without breaking login or Remember Username. Mobile device testing is outside the currently agreed scope.

## Evidence and limitations

Tester-reported screen observation; no case-specific defect screenshot has been verified in the evidence folder.


# DEF-AUTH-001 — Required security notice missing on login screen

| Field | Value |
|---|---|
| Project | Spa POS |
| Module | Authentication / Login |
| Requirement | AUTH-05 — Session handling |
| Related test case | AUTH-UI-05 |
| Type | UI / Requirement compliance |
| Severity | Minor (proposed) |
| Priority | Medium (proposed) |
| Status | OPEN |
| Detection date | 2026-10-13 |
| Environment | Spa POS web; browser/build not recorded |
| Evidence | Tester-reported manual observation; no new screenshot attached |
| Retest | NOT RUN |

## Steps to reproduce
1. Open Spa POS in a logged-out state.
2. Inspect the entire login screen.
3. Look for the security notice specified by AUTH-05.

## Expected result
The login screen displays the exact baseline notice: **“Không lưu thông tin đăng nhập trên thiết bị”**.

## Actual result
The required notice is absent. Login form and authentication otherwise remain functional according to the tester.

## Impact
The UI does not meet an explicitly documented security-information requirement. This observation **does not establish** that credentials are stored insecurely.

## Suggested fix
Add the notice to the login screen and verify its visibility at desktop, tablet and mobile sizes. Confirm the intended meaning of the notice alongside the Remember Username checkbox and session restoration logic to avoid misleading users.

## Retest criteria
The notice is visible and readable on the login screen at 375×667, 768×1024 and 1366×768; login and Remember Username behavior are unchanged. Record a screenshot and current build/commit.

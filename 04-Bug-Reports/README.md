# Bug Reports — Spa POS QA Portfolio

This folder follows a consistent professional QA defect-report structure. **Status and severity are evidence-based:** OPEN means unresolved as last reported; proposed severity/priority are not approved triage decisions.

## Current Authentication defects

| Defect ID | Summary | Test Case | Severity / Priority | Status | Evidence |
| --- | --- | --- | --- | --- | --- |
| [DEF-AUTH-001](DEF-AUTH-001-missing-login-security-notice-en.md) | Login security notice missing | AUTH-UI-05 | Minor / Medium (proposed) | OPEN | Chờ bổ sung |
| [DEF-AUTH-002](DEF-AUTH-002-remaining-login-attempts-not-shown-en.md) | Remaining attempts not shown | AUTH-VAL-04 | Minor / Medium (proposed) | OPEN | [Screenshot](../11-Test-Execution-Evidence/01-Authentication/AUTH-VAL-04-invalid-password.png) |
| [DEF-AUTH-003](DEF-AUTH-003-account-lockout-not-enforced-en.md) | No 15-minute lockout after five failures | AUTH-VAL-07 | Critical / High (proposed) | OPEN | [Failed-attempt state](../11-Test-Execution-Evidence/01-Authentication/DEF-AUTH-003-01-six-failed-attempts.png), [successful login](../11-Test-Execution-Evidence/01-Authentication/DEF-AUTH-003-02-seventh-attempt-success.png) |
| [DEF-AUTH-004](DEF-AUTH-004-offline-login-shows-invalid-credentials-en.md) | Offline failure mislabeled invalid credentials | AUTH-VAL-15 | Minor / Medium (proposed) | OPEN | Chờ bổ sung |

## Historical defects

See [Historical Defect Register](historical-defect-log.md). These records preserve previously reported fixes and unresolved observations; they are not automatically current OPEN bugs.

## Standard individual defect report format

1. **Defect details:** ID, title, project, module, requirement/test case, type, severity, priority, status, detected date, environment/build, owner, retest status and evidence.
2. **Preconditions and steps to reproduce:** clear numbered steps and safe test data.
3. **Expected vs Actual:** distinguish required behavior from what the tester observed.
4. **Impact / Risk:** concrete business or technical effect; avoid unsupported security claims.
5. **Suggested investigation / fix:** recommendation, not a confirmed root cause unless verified.
6. **Retest / Exit criteria and evidence limitations:** state whether a fix has actually been deployed and tested.

**Evidence convention:** Use direct GitHub links only for verified committed files. Otherwise use **Chờ bổ sung**. Keep tester-reported observations distinct from screenshot/SQL/API proof. Redact credentials, tokens and personal data.

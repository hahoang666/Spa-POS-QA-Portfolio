# DEF-AUTH-003 — Account not locked after five failed login attempts

## Defect details

| Field | Value |
| --- | --- |
| Defect ID | DEF-AUTH-003 |
| Project | Spa POS (web demo) |
| Module | Authentication / Login |
| Related requirement | AUTH-02 |
| Related test case | [AUTH-VAL-07](../03-Test-Cases/authentication-ui-validation-en.md) |
| Defect type | Security / Authentication control |
| Severity | Critical (proposed; pending triage) |
| Priority | High (proposed; pending triage) |
| Status | OPEN |
| Detected on | 2026-10-14 |
| Last reproduction | 2026-10-19 |
| Environment | Spa POS web; browser version and build SHA not recorded |
| Assigned to | Unassigned / not recorded |
| Retest status | NOT RUN — no confirmed fix |
| Evidence | [Six-failure screenshot](../11-Test-Execution-Evidence/01-Authentication/DEF-AUTH-003-01-six-failed-attempts.png); [seventh-attempt success](../11-Test-Execution-Evidence/01-Authentication/DEF-AUTH-003-02-seventh-attempt-success.png) |

## Preconditions

Active, authorized disposable test account; initially unlocked. **Initial failed-attempt counter state was not independently confirmed.**

## Steps to reproduce

1. Sign out and open the login screen.
2. For the same account, submit an incorrect password at least five times consecutively.
3. Observe whether a 15-minute lockout starts and whether a remaining-time notice appears.
4. Before 15 minutes have elapsed, submit the correct password.

## Expected result

After five failed attempts, the account is locked for 15 minutes; valid credentials are rejected during the lockout, and the user sees a lockout notice/countdown.

## Actual result

14 Oct: seven incorrect attempts followed by successful eighth attempt. 19 Oct reproduction: six incorrect attempts followed by successful seventh attempt, which opened Dashboard. No lockout message was reported.

## Impact / Risk

The required lockout control was not enforced in the tested flow, increasing password-guessing exposure. Other protections were not assessed.

## Suggested investigation / fix

Investigate backend failed-attempt tracking and server-side enforcement; do not rely on a browser-only counter.

## Retest / Exit criteria

**Not run; pending fix.** Once implemented, use a fresh test account, verify attempts 1–4, fifth-attempt lockout, correct-password denial during the 15-minute window, expiry, and reset behavior. Do not mark closed until retest.

## Evidence and limitations

Attempt counts and sequence are tester-reported; screenshots capture individual visible states, not a continuous attempt log. No server logs or post-fix retest.

### Reproduction history

- **2026-10-14:** 7 wrong-password submissions, followed by successful login with correct password on attempt 8.
- **2026-10-19:** 6 wrong-password submissions, followed by successful login with correct password on attempt 7.
- **Current disposition:** OPEN. No fix has been verified; AUTH-VAL-08 counter-reset testing remains blocked pending implementation.


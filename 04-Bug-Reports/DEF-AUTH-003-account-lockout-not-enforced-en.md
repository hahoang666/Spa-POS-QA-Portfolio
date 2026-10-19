# DEF-AUTH-003 — Account not locked after five failed login attempts

| Field | Value |
|---|---|
| Project | Spa POS |
| Module | Authentication |
| Requirement | AUTH-02 |
| Test case | AUTH-VAL-07 |
| Defect type | Security / Authentication control |
| Severity | Critical (proposed) |
| Priority | High (proposed) |
| Status | OPEN |
| Detected | 2026-10-14 |
| Environment | Spa POS web; browser/build not recorded |
| Evidence | Tester-reported manual execution; no backend logs or video attached |
| Retest | NOT RUN |

## Preconditions
A dedicated active test account with a valid password, initially unlocked and with a reset failed-attempt counter. **Initial counter state was not independently verified**.

## Steps to reproduce
1. Open the login page in a logged-out state.
2. Enter the same test username and an incorrect password for seven consecutive login attempts.
3. Observe the messages and any lockout or countdown after attempt five.
4. On attempt eight, enter the correct password.

## Expected result
After five consecutive failed attempts, the account is locked for 15 minutes. Login with the correct password during the lock period is blocked; the UI displays lockout and remaining time (AUTH-02).

## Actual result
Attempts 1–7 each displayed “Tên đăng nhập hoặc mật khẩu không đúng.” with no lockout message or remaining lock time. Attempt 8 with the correct password succeeded. The observed flow does not enforce the required lockout.

## Impact
The documented account lockout protection is ineffective in the observed scenario, increasing exposure to repeated password guessing. Other rate limits or protections were not assessed.

## Suggested investigation / fix
Inspect the authentication flow and server-side enforcement of failed-attempt tracking and lock expiration. Ensure lockout cannot be bypassed by a correct password during the lock window, and return safe lockout status to the UI. Avoid client-only enforcement.

## Retest criteria
With an isolated test account and confirmed clean counter, attempts 1–4 are rejected; attempt 5 activates a 15-minute lock; correct and incorrect passwords remain blocked during the lock; countdown is visible; after expiration, valid credentials work. Capture timestamps, network responses and build version without exposing secrets.

## Additional reproduction — 19 October 2026

A new manual execution was reported: six consecutive invalid-password attempts showed the same generic error, followed by successful login with the correct password on attempt seven and redirection to the Dashboard. **Result: FAIL / REPRODUCED; defect remains OPEN.** This is a new observation, separate from the original 14 October attempt sequence above. Screenshot filenames are recorded in [the Authentication evidence register](../11-Test-Execution-Evidence/01-Authentication/README.md), but PNG upload is pending. The count of failed attempts is based on tester notes; the screenshots do not show a counter or full attempt history. No fix retest was performed.

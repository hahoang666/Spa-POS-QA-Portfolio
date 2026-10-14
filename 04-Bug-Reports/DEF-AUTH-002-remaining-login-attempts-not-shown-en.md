# DEF-AUTH-002 — Remaining login attempts not displayed

| Field | Value |
|---|---|
| Project | Spa POS |
| Module | Authentication / Login |
| Requirement | AUTH-01 |
| Test case | AUTH-VAL-04 |
| Type | Validation feedback / Requirement compliance |
| Severity | Minor (proposed) |
| Priority | Medium (proposed) |
| Status | OPEN |
| Date | 2026-10-14 |
| Environment | Spa POS web; browser and build not recorded |
| Evidence | Tester screenshot: image(10).png (conversation attachment; not yet uploaded to repository) |
| Retest | NOT RUN |

## Steps to reproduce
1. Open the login screen in a logged-out state.
2. Enter test username `abc123` and an invalid test password.
3. Click Login once.
4. Observe the validation feedback and remaining attempt count.

## Expected result
Reject invalid credentials, show a generic invalid-credentials message without disclosing which field is wrong, and display the number of remaining login attempts per AUTH-01.

## Actual result
The login is rejected; the generic message “Tên đăng nhập hoặc mật khẩu không đúng.” is shown and the Password field is outlined red. **The remaining login attempts are not displayed.**

## Impact
The login screen fails to provide required feedback about remaining attempts. This does not establish that lockout or attempt counting is broken; test AUTH-VAL-07 separately.

## Suggested fix
Show a remaining-attempt counter after failed login in a manner consistent with server-side lockout state. Avoid revealing whether the username exists. Confirm AUTH-02 lockout rules and retest.

## Retest criteria
After an invalid login, the generic error and accurate remaining-attempt count appear. Verify successive failures and lockout with an isolated test account. Capture screenshot and build identifier.

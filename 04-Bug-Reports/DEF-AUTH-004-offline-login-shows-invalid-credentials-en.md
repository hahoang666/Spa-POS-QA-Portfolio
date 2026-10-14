# DEF-AUTH-004 — Offline login displays incorrect credentials error

- **Requirement:** AUTH-01
- **Related test case:** AUTH-VAL-15
- **Module:** Authentication / Login
- **Severity:** Minor (proposed)
- **Priority:** Medium (proposed)
- **Status:** OPEN
- **Test date:** 2026-10-14
- **Environment:** Spa POS web, Chrome DevTools Network Offline; build not recorded

## Preconditions
- Spa POS login screen is displayed.
- An authorized test account has valid credentials.
- Chrome DevTools is open, with Network throttling set to **Offline**.

## Steps to reproduce
1. Open the Spa POS login page and sign out if needed.
2. Open DevTools → Network and select **Offline**, keeping DevTools open.
3. Enter valid test-account credentials.
4. Click **Login** and observe the error.
5. Click **Login** again and observe button behavior.

## Expected result
Display an appropriate error for a network/server connectivity failure, prevent unauthorized access, and restore the Login button so the user can retry (AUTH-VAL-15 original expected result).

## Actual result
The app denies Dashboard access but displays **“Tên tài khoản hoặc mật khẩu không đúng”** (incorrect username or password) even though the credentials are valid and the network is simulated offline. The Login button remains responsive and allows retry.

## Impact
Users are incorrectly told their credentials are wrong when the request cannot reach the server, making troubleshooting harder.

## Additional observation
In a separate attempt, the tester closed DevTools after selecting Offline and login succeeded. The Offline condition was **not verified** after closing DevTools; this is not evidence of an authentication bypass.

## Scope / evidence
Manual tester report; no secrets or tokens captured. No production outage was induced.

## Suggested fix
Differentiate network/transport errors from invalid credentials in the login error handler. Show a connectivity-specific message and preserve retry behavior.

## Retest
Repeat AUTH-VAL-15 with DevTools open and Offline, verify appropriate message, denied Dashboard access and responsive retry button; then restore No throttling.

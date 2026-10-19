# Authentication & Role Authorization — Evidence Register (2026-10-19)

**Project:** Spa POS web demo  
**Scope:** AUTH-VAL-04, DEF-AUTH-003 / AUTH-VAL-07, AUTH-VAL-10, AUTH-VAL-13, and Employee payment authorization  
**Source:** Screenshots and tester observations supplied on 19 October 2026.  
**Important:** Image filenames below are **pending binary upload**; this register records the observations, not a claim that PNG files have been committed. Test credentials and JWTs must not be published.

| ID | Evidence filename (pending upload) | Observation | Assessment |
| --- | --- | --- | --- |
| AUTH-VAL-04 | `AUTH-VAL-04-invalid-password.png` | Login form displays red password border and “Tên đăng nhập hoặc mật khẩu không đúng.” | PASS for invalid-credentials UI feedback |
| DEF-AUTH-003 / AUTH-VAL-07 | `DEF-AUTH-003-01-six-failed-attempts.png` | Tester reports six consecutive invalid-password submissions; screenshot shows generic invalid-credentials error, no lockout notice | FAIL, combined with next screenshot and execution sequence |
| DEF-AUTH-003 / AUTH-VAL-07 | `DEF-AUTH-003-02-seventh-attempt-success.png` | Tester reports successful login using valid password on attempt seven, immediately after six failures; screenshot shows authenticated Dashboard | FAIL — lockout requirement not enforced in reported sequence |
| AUTH-VAL-10 | `AUTH-VAL-10-login-after-logout.png` | Login screen shown after logout | PARTIAL — Back, reload and new-tab denial not demonstrated by screenshot |
| Remember username | `AUTH-REMEMBER-username-checked.png` | Username is present while “Remember username” is checked | UI observation only — not Session Storage evidence |
| ROLE/API | `ROLE-API-employee-payment-403.png` | Postman POST to `/rest/v1/rpc/create_payment_v2` returned HTTP 403, SQLSTATE 42501, “FORBIDDEN: missing permission PAYMENT_CREATE” | PASS conditional on independently confirming request used Employee JWT |

## DEF-AUTH-003 reproduction — 19 October 2026

- **Requirement:** Five consecutive incorrect passwords trigger a 15-minute account lockout; valid password must be rejected during lockout.
- **Execution reported:** Six incorrect attempts for the same account, then a seventh attempt with the correct password.
- **Actual:** Generic invalid-credentials message remained visible after the sixth attempt; seventh attempt succeeded and opened the Dashboard.
- **Result:** **FAIL / REPRODUCED**, defect remains **OPEN**. Severity and priority remain proposals pending triage.
- **Evidence limitation:** Individual attempts, server logs, exact elapsed time, initial failure-counter state, and browser/build SHA were not independently captured. The screenshots substantiate the visible states; the attempt sequence comes from the tester's execution notes.
- **Retest:** Not performed after a fix.

## Outstanding evidence

1. **AUTH-VAL-10:** Screenshot or short screen recording showing logout → browser Back → reload/new tab → login required; include timestamps.
2. **AUTH-VAL-13:** DevTools Application → Local Storage / Session Storage and cookies **after logout**, with JWTs, tokens and secrets fully redacted; confirm absence of plaintext passwords and persisted authentication tokens. The remember-username screenshot does **not** cover this.
3. **DEF-AUTH-003:** Optional stronger evidence: timestamped attempt log or screen recording showing five failed submissions and valid-credential rejection expected vs observed; avoid disclosing the actual password.
4. **ROLE/API:** Confirm the request was authenticated as an Employee test account and attach sanitized Authorization setup / role evidence. Check that rejected request created no payment or cashbook transaction using read-only SQL.
5. **Environment metadata:** browser/version, app build/commit SHA, execution date/time, test account role, and related test case links.

## Evidence naming and destination

Upload the six PNGs to `11-Test-Execution-Evidence/01-Authentication/`. This register can then be updated to link directly to committed image paths. Do not treat application UI screenshots in `10-Screenshots/` as test-execution proof by themselves.

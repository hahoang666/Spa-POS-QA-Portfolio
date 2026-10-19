# Spa POS — Test Summary Report (TSR)

| Report metadata | Value |
| --- | --- |
| Project | Spa POS — Web Demo |
| Report ID | TSR-SPAPOS-2026-10-19-01 |
| Report type | Historical / cumulative QA execution summary; **not release sign-off** |
| Reporting cut-off | 19 October 2026 |
| Test period covered | 8–19 October 2026 (according to recorded execution dates) |
| Prepared by | QA portfolio author (individual project with AI assistance) |
| Test levels | Functional, integration, API, authorization/security checks, regression and selected E2E |
| System under test | [Spa POS web demo](https://hahoang666.github.io/Spa-pos-pp/) |
| Frontend source | [Spa-pos-pp](https://github.com/hahoang666/Spa-pos-pp) |
| Backend | Supabase PostgreSQL / RPC / RLS |
| Tools | Chrome / DevTools, Postman, Supabase SQL, GitHub |
| Browser / OS / build SHA | Not consistently captured; **Chờ bổ sung** |
| Approval / release decision | Not requested or granted; **NOT READY FOR SIGN-OFF** |

## 1. Executive summary

This report consolidates tester-reported executions and committed documentation for the Spa POS web demo. Authentication includes four known unresolved defects; payment idempotency regression shows successful recorded checks but still has untested failure/retry paths. Several historical API and SQL executions lack committed request/response screenshots. **Do not interpret historical PASS statuses as a full current-build regression or production release certification.**

**Overall assessment:** **PARTIAL COVERAGE / OPEN DEFECTS / FURTHER TESTING REQUIRED.** Existing results demonstrate tested flows, not approval to release.

## 2. Scope and exclusions

**In scope:**

- Authentication: login validation, logout/session behavior, remember-username and browser-side storage, account lockout and offline error handling.
- Sales E2E: cash, bank transfer, partial payment and debt collection with invoice/payment/cashbook reconciliation.
- Payment API: valid and invalid amounts, idempotency key behavior, authorization and duplicate-click protection.
- Historical shift management, invoice cancellation, customer/payment-method data sync, employee sync, and selected HR/attendance observations.

**Not completed / out of scope for this reporting cycle:**

- Full mobile-device testing (no mobile product build under test; desktop browser responsive checks are not equivalent).
- Account lockout **retest** and failed-attempt counter reset until the related fix is available.
- Lost-response/timeout payment retry, invoice-creation idempotency, direct legacy payment RPC denial via Postman.
- Complete fresh regression of every module on one identified build; production security/performance certification.

## 3. Execution summary — source-specific counts

Counts are taken from distinct source files and are **not additive** across overlapping registers.

| Source / test suite | Total recorded rows | PASS | FAIL | Other statuses | Evidence condition |
| --- | ---: | ---: | ---: | --- | --- |
| [Authentication](../03-Test-Cases/authentication-ui-validation-en.md) | 21 | 15 | 4 | 1 PASS WITH OBSERVATION; 1 NOT RUN (execution blocked pending lockout fix) | 7 cases with linked images; 14 **Chờ bổ sung** |
| [Sales E2E](../03-Test-Cases/sales-e2e-en.md) | 4 | 4 (includes 1 PASS RETEST) | 0 | — | 4 **Chờ bổ sung** |
| [DEF-SALE-013 retest/regression](../03-Test-Cases/def-sale-013-retest-2026-10-14.md) | 11 | 10 | 0 | 1 NOT RUN / SKIPPED | 11 **Chờ bổ sung** |
| [Postman API historical](../03-Test-Cases/postman-api-executions.md) | 12 | 11 (includes 1 RETEST PASS; 1 tester-reported) | 0 | 1 NOT VERIFIED | 12 **Chờ bổ sung** |
| [Legacy historical snapshot](../03-Test-Cases/legacy-execution-samples-2026-10-10.md) | 25 | 21 (some reported / rule-only) | 4 historical | — | 25 **Chờ bổ sung** |

**Counting conventions:** A `PASS (RETEST)` is included in PASS. `PASS WITH OBSERVATION` is shown separately. `NOT VERIFIED`, `NOT RUN`, and `NOT RUN / SKIPPED` are not PASS. Legacy FAIL results are historical and may have been superseded by later retests. Postman and Legacy repeat shift/payment IDs, so there is **no combined execution count or overall pass rate** in this report.

## 4. Key test outcomes

| Area | Verified or reported observation | Disposition |
| --- | --- | --- |
| Authentication — logout | Back did not restore Dashboard; F5 and new tab required login | PASS; screenshots for Back/Reload, new-tab outcome tester-reported |
| Authentication — browser storage | No visible Supabase auth token after logout; Session Storage/Cookies empty; tester inspected `spa_pos_state_v1` for password/access/refresh token | PASS within browser-storage scope; not proof of server-side revocation |
| Authentication — lockout | Correct password still accepted after more than five reported incorrect attempts | FAIL / DEF-AUTH-003 OPEN; **no fix retest** |
| Authentication — feedback/offline | Remaining-attempt counter absent; offline failure displayed invalid-credentials message; login security notice absent | FAIL; see DEF-AUTH-001/002/004 |
| Sales E2E — cash/bank/partial/debt | Recorded invoice/payment/cashbook reconciliation and persistence checks; four Sales E2E rows PASS | PASS as historically reported; case-specific screenshots pending |
| Payment idempotency | Same UUID/payload returns same payment; conflicting payload 409; double-click regression reported no duplicates | 10 recorded retest/regression PASS; defect **not finally accepted/closed** |
| Shift and cancellation API | Shift rules and invalid invoice cancellation error mapping retest recorded | Historical PASS; sanitized API evidence pending |
| HR employee sync | Four FAIL in 10 Oct legacy snapshot; later retests reported PASS | Do not treat 10 Oct FAIL as confirmed current bugs |

## 5. Defect summary

### Authentication — unresolved defects

| Defect | Description | Severity / Priority | Status | Retest |
| --- | --- | --- | --- | --- |
| [DEF-AUTH-001](../04-Bug-Reports/DEF-AUTH-001-missing-login-security-notice-en.md) | Security notice missing | Minor / Medium (proposed) | OPEN | NOT RUN |
| [DEF-AUTH-002](../04-Bug-Reports/DEF-AUTH-002-remaining-login-attempts-not-shown-en.md) | Remaining attempts not displayed | Minor / Medium (proposed) | OPEN | NOT RUN |
| [DEF-AUTH-003](../04-Bug-Reports/DEF-AUTH-003-account-lockout-not-enforced-en.md) | No 15-minute lockout after five failed attempts | Critical / High (proposed) | OPEN | NOT RUN — pending fix |
| [DEF-AUTH-004](../04-Bug-Reports/DEF-AUTH-004-offline-login-shows-invalid-credentials-en.md) | Offline failure shown as wrong credentials | Minor / Medium (proposed) | OPEN | NOT RUN |

### Other relevant defect / regression status

- **DEF-SALE-013 (duplicate payments):** historical defect, currently **OPEN / FIXED PENDING FINAL ACCEPTANCE**; 10 PASS checks and 1 SKIPPED in the recorded retest/regression suite. Outstanding timeout/lost-response and invoice-idempotency scenarios. See [test cases](../03-Test-Cases/def-sale-013-retest-2026-10-14.md).
- Other shift, invoice, product, HR and attendance issues are documented in the [historical defect register](../04-Bug-Reports/historical-defect-log.md). Their historical statuses must not be added to the current OPEN count without new verification.
- **Known OPEN defects in current Authentication register: 4.** This is **not** a project-wide current defect count; triage status for all other modules is incomplete.

## 6. Evidence, traceability and data integrity

**Available:** GitHub Test Case registers, linked Authentication screenshots, defect reports, historical SQL observations, recorded payment/retest results and source commits where available.

**Pending:** sanitized Postman requests/responses and SQL outputs for historical cases; case-specific Sales E2E screenshots; a consistent test environment/build record; fresh validation for unverified negative-path data integrity. `Chờ bổ sung` in Evidence is a documentation gap, **not** proof that a test was never executed.

**Traceability:** Requirement IDs and Test Case IDs appear in the test case registers. Defect IDs appear in failed cases where known. The same historical test may occur in more than one file; use case ID plus execution date/build to distinguish runs.

## 7. Risks, blockers and limitations

1. **High release risk:** account lockout requirement not enforced in observed Authentication flow (DEF-AUTH-003; severity Critical proposed).
2. **Financial integrity risk:** payment retry behavior during lost response/timeout and invoice creation idempotency not fully verified; DEF-SALE-013 not formally accepted.
3. **Coverage blocker:** AUTH-VAL-08 failed-attempt counter reset cannot be meaningfully tested until counting/lockout behavior is implemented or fixed.
4. **Evidence quality:** many historical results are tester-reported without attached raw screenshots, SQL exports or Postman collections.
5. **Environment traceability:** no consistent browser/build SHA across all historical executions; this report cannot establish one-build end-to-end coverage.
6. **Platform scope:** web desktop/browser responsive testing only; no mobile-device sign-off.

## 8. Exit criteria and QA recommendation

| Exit criterion | Current assessment |
| --- | --- |
| Critical authentication defects resolved and retested | **NOT MET** |
| Payment integrity retry scenarios fully verified | **NOT MET** |
| No untriaged high-impact failures in release scope | **NOT CONFIRMED** |
| All planned scope executed or documented as excluded/blocked | **PARTIALLY MET** |
| Reproducible, linked evidence for key cases | **PARTIALLY MET** |
| Test environment and build identified for final regression | **NOT MET** |

**QA recommendation:** Continue testing and evidence consolidation. **No release sign-off is issued.** Before a release decision, confirm the build, triage/fix the Authentication defects, complete payment-integrity negative scenarios, execute targeted retests and one current-build regression, and update the report with actual dated outcomes.

## 9. Next actions

1. Attach remaining case-specific Evidence without rewriting historical Actual Results.
2. After a confirmed lockout fix, execute AUTH-VAL-07 retest and AUTH-VAL-08 counter-reset test; do not claim PASS beforehand.
3. Complete DEF-SALE-013 timeout/retry and direct legacy-RPC denial coverage.
4. Reconcile negative-path financial writes in SQL and record a known build SHA.
5. Refresh the defect register and publish a new dated Test Summary version after regression.

## 10. Sign-off / review

| Role | Name / decision | Date |
| --- | --- | --- |
| QA preparer | Individual portfolio author | 2026-10-19 |
| QA reviewer | Not assigned | — |
| Developer / defect triage approval | Not recorded | — |
| Release approver | Not applicable / not requested | — |

---

**Report integrity note:** This is a learning/portfolio project report, not a claim of commercial QA employment, independent production audit or production readiness. All figures are based on the linked historical registers and should be recalculated when source statuses change.

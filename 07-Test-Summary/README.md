# 07 — Test Summary Reports

This folder contains QA test execution summaries and risk assessments for the Spa POS web demo. A **Test Summary Report (TSR)** communicates what was tested, results, unresolved defects, evidence coverage, limitations and QA recommendations. It is not a release approval by itself.

## Available report

- **[TSR-SPAPOS-2026-10-19-01 — Spa POS Historical Test Summary](historical-test-summary.md)** — reporting cut-off 19 October 2026; Authentication, Sales E2E, payment idempotency regression, historical Postman/API and legacy execution. Includes source-specific counts, open defects, risks, exit criteria and sign-off status.

## Reporting conventions

- Keep execution counts **per source suite**; Postman and Legacy share Test Case IDs and must not be summed as independent runs.
- Do not change historical FAIL to PASS based on later fixes without a dated retest result.
- Mark missing attachments **Chờ bổ sung**. Missing evidence is not the same as NOT RUN.
- Distinguish **OPEN**, **RETEST PASS**, **NOT RUN**, **NOT VERIFIED** and **PASS WITH OBSERVATION**.
- Mark severity/priority as *proposed* until approved in defect triage.
- Record environment/build SHA and reviewer/approval only when known. This is a personal learning portfolio, not a commercial project sign-off.

## Recommended format for future test cycles

Report ID and version; reporting period/build; scope and exclusions; test environment; execution summary; defects and severity; evidence/traceability; risks and blockers; exit criteria; QA recommendation; review/sign-off. Create a new dated report for a new tested build instead of overwriting prior execution history.

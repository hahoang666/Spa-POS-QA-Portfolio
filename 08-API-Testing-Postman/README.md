# 08 — API Testing with Postman (Spa POS)

**Status:** Reconstructed historical QA materials from conversations and documented execution (8–14 October 2026). **Not an original Postman export or a new execution.** This folder is suitable for importing a safe template and learning to reproduce tests with fresh QA fixtures.

## Files

| File | Purpose |
| --- | --- |
| [Spa-POS-QA.postman_collection.json](Spa-POS-QA.postman_collection.json) | Importable Postman Collection v2.1; login, shift, payments, payment idempotency and invoice cancellation examples |
| [Spa-POS-QA.postman_environment.json](Spa-POS-QA.postman_environment.json) | Importable environment template; credentials/tokens intentionally **blank** |
| [historical-request-response-examples.json](historical-request-response-examples.json) | Sanitized **reconstructed** historical requests, HTTP codes, SQLSTATE and results |
| [api-testing-guide-vi.md](api-testing-guide-vi.md) | Step-by-step Vietnamese setup, request anatomy, status codes, workflows, evidence guidance |
| [sql-verification-queries.md](sql-verification-queries.md) | Read-only Supabase SQL checks, RPC signature/permission checks and negative-path verification |
| [qa-execution-reviewed-en.md](qa-execution-reviewed-en.md) | Reviewed historical API/SQL execution record (preserves original case IDs) |
| [legacy-api-report-2026-10-10.md](legacy-api-report-2026-10-10.md) | Archived snapshot from 10 October; not current-build sign-off |

## Quick start

1. Download the two `.json` files from GitHub (Raw → Save As) and import them into Postman.
2. Select **Spa POS QA - Template (NO SECRETS)**; fill environment values **locally only**.
3. Start with Login. For RPCs, use an authorized QA account and fresh disposable shift/invoice fixtures.
4. Inspect HTTP response and verify DB state using the SQL guide. **Do not run payment/cancel requests against historical invoice IDs.**
5. Add screenshots/exports only after removing sensitive headers, access/refresh tokens, passwords, keys and personal information.

## Execution provenance and safety

- Historical observations: shift open/duplicate/negative/close, full/partial payments, invoice cancellation, authorization and `create_payment_v2` idempotency.
- Legacy `create_payment` permissions may now be revoked; confirm deployed signature and privileges before testing.
- The collection includes **mutating requests**. Their presence is a reusable template, **not** evidence they have been rerun or are safe to run without isolated QA data.
- Missing screenshots and original raw Postman exports remain **Chờ bổ sung**. Never publish `service_role` keys, JWT, passwords or active authenticated environment exports.
- Test case sources: [Postman API](../03-Test-Cases/postman-api-executions.md) and [DEF-SALE-013 Retest](../03-Test-Cases/def-sale-013-retest-2026-10-14.md).

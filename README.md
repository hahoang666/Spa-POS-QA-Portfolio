# Spa POS — QA Testing Portfolio

**Manual Testing | Functional Testing | End-to-End Testing | SQL Validation**

> Personal learning project built with AI assistance. Historical test results are documented; they are not a fresh certification of the current build.

## Application under test
- [Live Spa POS demo](https://hahoang666.github.io/Spa-pos-pp/)
- [Application source code](https://github.com/hahoang666/Spa-pos-pp)
- Web application with Supabase/PostgreSQL backend.

## QA scope
Invoices, payments, returns/refunds, appointments, products, employee management, attendance, permissions and cashbook.

## QA documentation
- [Test Plan](01-Test-Plan/)
- [Test Scenarios](02-Test-Scenarios/)
- [Test Cases](03-Test-Cases/)
- [Defect Log](04-Bug-Reports/)
- [Retest Evidence](05-Retest-Evidence/)
- [Regression Report](06-Regression-Testing/)
- [Test Summary](07-Test-Summary/)
- [API Testing](08-API-Testing-Postman/)
- [SQL Validation](09-SQL-Validation/)
- [Screenshots](10-Screenshots/)
- [Vietnamese Reference](Vietnamese-Reference/)

## Selected historical tests
- HD00000004 — invoice cancellation status synchronized after frontend fix.
- HD00000009 — return reversal, refund and cashbook validation.
- EMP000002 — employee edit and linked account regression.
- APPT-DATE-01 to 03 — appointment date filter tests.
- CARD-01 to 05 — card cashbook tests.

## Test result integrity
PASS = previously reported as passed; BLOCKED = execution prevented; NOT VERIFIED = evidence or current-build verification not available. Test results should not be represented as newly executed.

## Project attribution
Personal project developed and tested with ChatGPT assistance. No employer or production testing experience is claimed. Do not commit credentials, private customer information or raw access tokens.

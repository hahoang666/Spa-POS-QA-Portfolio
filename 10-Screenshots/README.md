# Spa POS — Application Screenshots

Screenshots of the Spa POS web demo used for hands-on QA practice. **Screenshots document the interface; they do not by themselves establish a test PASS result.**

| No. | File | Screen | What it shows |
| --- | --- | --- | --- |
| 01 | `01-login-screen.png` | Login | Username, password, remember-username, login button |
| 02 | `02-dashboard-overview.png` | Dashboard | Revenue, invoices, customer count, outstanding debt, charts |
| 03 | `03-appointment-calendar.png` | Appointments | Calendar, date filters, appointment list |
| 04 | `04-cashier-sales.png` | Cashier / Sales | Product selection, invoice details, VAT and payment inputs |
| 05 | `05-invoice-list.png` | Invoice list | Invoice filters, payment statuses and actions |
| 06 | `06-sales-return.png` | Sales returns | Return list, original invoice reference, refund information, return status |
| 07 | `07-customer-list.png` | Customers | Customer profiles, outstanding balance and available actions |
| 08 | `08-treatment-cards.png` | Treatment cards | Treatment card list and empty state |
| 09 | `09-services-and-packages.png` | Services & packages | Service/package catalog, sessions, duration, price and VAT |
| 10 | `10-product-list.png` | Products | Product catalog, inventory display, price and VAT |
| 11 | `11-price-settings.png` | Price settings | Product/service prices and edit-price actions |

**Upload status:** Image files are prepared for upload, but binary PNG files are not yet committed to this repository. The table is an index, not an assertion that these paths exist.

### QA review notes

- The product-list screenshot displays `NaN` in the inventory/on-hand field. This is a **potential display/data-handling defect** to investigate, not a confirmed defect.
- Empty treatment-card and appointment views show the captured application state only; they do not prove an absence of functionality.
- Screenshots contain demonstration/test data. Review for personal or sensitive information before publication.

For execution results, defect details, and retest evidence, refer to the relevant QA documentation elsewhere in this repository.

## Additional screens (12–20)

| No. | Planned image filename | Screen | Visible information |
| --- | --- | --- | --- |
| 12 | `12-employee-profiles.png` | Employee profiles | Employee records, roles, account linkage, status |
| 13 | `13-attendance.png` | Attendance | GPS check-in/out, attendance history, filters and export |
| 14 | `14-monthly-attendance.png` | Monthly attendance | Monthly calendar, attendance summary and export |
| 15 | `15-cashbook-funds.png` | Cashbook: cash | Opening balance, receipts, expenses and closing balance; other fund tabs visible |
| 16 | `16-fund-summary.png` | Total funds | Consolidated opening balance, receipts, expenses and closing balance |
| 17 | `17-sales-invoice-report.png` | Sales invoice report | Invoice numbers, dates, customers, pre-VAT amount, VAT and total |
| 18 | `18-vat-summary-report.png` | VAT summary | Output VAT, input VAT and VAT payable |
| 19 | `19-business-performance-report.png` | Business performance | Sales quantity, gross sales, deductions and net revenue by day |
| 20 | `20-accounts-receivable-and-ledger-report.png` | Accounts receivable and ledger report | Customer outstanding balances and cash/bank ledger report section |

**Status:** The image files above are prepared locally for manual upload; the README update does not mean PNGs have been committed to GitHub.

**QA observation:** In the employee-profile screenshot, a record named `QA Tester Inactive` appears with status `Hoạt động` (Active). This is a point for further verification, not a confirmed defect. The ledger report in screenshot 20 displays zeros; reconcile the report period and data source before drawing a conclusion.

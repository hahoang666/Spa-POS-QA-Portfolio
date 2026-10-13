# Supabase / PostgreSQL Validation — Spa POS

These are **read-only example queries** based on previously confirmed table/column names. They are provided for reproducibility; **they were not all re-run as a batch** when this document was published. Run only in an authorized test database.

## Historical database findings
- On 8 October 2026, the new `cashbook_transactions`, `debt_collections`, `debts`, `funds` and `shifts` tables were reported with RLS enabled.
- HD00000004: status CANCELLED, payment_status CANCELLED, paid_amount 0, due_amount 0, reason recorded; frontend initially displayed stale debt status.
- HD00000008: total 110,000; paid 70,000; due 40,000; PARTIALLY_PAID.
- HD00000009: return reversed and refunded 110,000; cashbook OUT transaction for refund.
- Attendance: a check-in record was created on 12 October 2026; full checkout E2E remained unverified in recovered evidence.

## Read-only SQL examples

```sql
-- Invoice status and outstanding balance
select id, code, status, payment_status,
       total_amount, paid_amount, due_amount
from public.invoices
where code in ('HD00000004','HD00000008','HD00000009');

-- Payments associated with selected invoices
select p.id, p.invoice_id, p.payment_method_id, p.amount, p.shift_id
from public.payments p
where p.invoice_id in (4, 8, 9)
order by p.invoice_id, p.id;

-- Cashbook entries associated with invoice 9
select id, fund_id, direction, amount, reference_type, reference_id
from public.cashbook_transactions
where (reference_type = 'INVOICE' and reference_id = 9)
   or (reference_type = 'RETURN' and reference_id = 1)
order by id;

-- Check RLS flags in the public schema
select tablename, rowsecurity
from pg_tables
where schemaname = 'public'
  and tablename in ('shifts','funds','cashbook_transactions','debts','debt_collections');
```

**Caution:** The cashbook query's return reference type is illustrative; actual refund entries previously used a RETURN_REFUND category and may use a different reference_type. Verify schema and data before treating a zero-row result as a defect.

## Validation principle
A successful HTTP response is not enough for financial flows: check that invoice totals, payment records and cashbook transactions remain consistent, including rejected operations.

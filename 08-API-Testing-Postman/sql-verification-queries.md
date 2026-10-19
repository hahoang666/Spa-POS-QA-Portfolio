# SQL Verification — Safe Read-Only Templates

These are **templates**, not previously executed SQL result exports. Run only in an authorized Supabase SQL Editor session. Replace fixture IDs and confirm column names against the deployed schema.

## Invoice and payment balances

```sql
select id, code, status, payment_status, total_amount, paid_amount, due_amount
from public.invoices
where id = 8; -- historical fixture; replace with a NEW test invoice
```

```sql
select id, invoice_id, payment_method_id, amount, reference_no, shift_id, paid_at
from public.payments
where invoice_id = 8
order by id;
```

## Payment and cashbook counts

```sql
select count(*) as payment_count, coalesce(sum(amount), 0) as payment_total
from public.payments
where invoice_id = 8;
```

```sql
-- First inspect cashbook schema/columns and identify linkage to payment/invoice.
select column_name, data_type
from information_schema.columns
where table_schema='public' and table_name='cashbook_transactions'
order by ordinal_position;
```

**Do not assume a cashbook foreign-key column exists.** Historical counts were reconciled with the deployed schema; use the actual linkage/reference fields before writing a per-invoice query.

## Shift verification

```sql
select id, status, opening_cash
from public.shifts
where id = 4; -- historical fixture; replace
```

```sql
select column_name, data_type
from information_schema.columns
where table_schema='public' and table_name='shifts'
order by ordinal_position;
```

## Verify RPC signature and permissions

```sql
select p.proname, pg_get_function_identity_arguments(p.oid) as args,
       has_function_privilege('authenticated', p.oid, 'EXECUTE') as authenticated_can_execute,
       has_function_privilege('anon', p.oid, 'EXECUTE') as anon_can_execute
from pg_proc p
join pg_namespace n on n.oid = p.pronamespace
where n.nspname='public'
  and p.proname in ('open_shift','get_current_shift','close_shift',
                   'create_payment','create_payment_v2','cancel_invoice')
order by p.proname;
```

## Negative-path integrity procedure

1. Query and record invoice/payment/cashbook state **before** the rejected request.
2. Send one authorized negative test in Postman; record sanitized HTTP status, SQLSTATE, message and time.
3. Re-run the **same SQL** afterward; compare balances, counts and IDs.
4. Mark full integrity PASS only when the relevant records are unchanged. Otherwise report **API rule PASS / DB integrity pending**.

Never publish JWTs, secrets, raw credentials or unredacted customer/employee information.

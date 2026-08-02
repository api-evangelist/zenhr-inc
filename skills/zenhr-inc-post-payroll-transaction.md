---
name: Post a financial transaction to payroll
description: Create an employee financial transaction and reconcile it against branch salaries.
api: openapi/zenhr-inc-openapi.yml
operations: [listEmployees, createEmployeeFinancalTransactions, listBranchSalaries]
---

# Post a financial transaction to payroll

Use the ZenHR API (`https://app.zenhr.com/api/v3`) to add a one-off financial transaction (allowance, deduction, bonus) and check it against payroll.

## Auth
OAuth 2.0 Authorization Code + PKCE, `Authorization: Bearer <access_token>`. Requires the `read:financial_transaction` scope plus write authority. Respect rate limits (15 req / 4s production).

## Steps
1. `listEmployees` (`GET /branches/{branch_id}/employees`) — resolve the `employee_id`.
2. `createEmployeeFinancalTransactions` (`POST /branches/{branch_id}/employees/{employee_id}/financial_transactions`) — post the transaction (use the `_requests` variant if it must route through approval).
3. `listBranchSalaries` (`GET /branches/{branch_id}/salaries`) — confirm the transaction is reflected in the branch payroll run.

## Notes
For idempotent external references, use the `integration_maps` lookups (`GET/PATCH /branches/{branch_id}/integration_maps/financial_transaction/{global_id}`) to bind your own global id. No idempotency-key header is documented — de-duplicate via integration_maps. Errors: see `errors/zenhr-inc-problem-types.yml`.

---
name: Request time off for an employee
description: Submit a time-off transaction request for an employee in a ZenHR branch.
api: openapi/zenhr-inc-openapi.yml
operations: [whoAmI, listEmployees, createTimeoffTransactionRequest]
---

# Request time off for an employee

Use the ZenHR API (`https://app.zenhr.com/api/v3`) to file a time-off request.

## Auth
OAuth 2.0 Authorization Code + PKCE, `Authorization: Bearer <access_token>`. Requires the `read:employee` and `read:timeoff_transaction` scopes. Respect rate limits (15 req / 4s production).

## Steps
1. `whoAmI` (`GET /who_am_i`) — confirm the authenticated principal and accessible branches.
2. `listEmployees` (`GET /branches/{branch_id}/employees`) — resolve the `employee_id`.
3. `createTimeoffTransactionRequest` (`POST /branches/{branch_id}/employees/{employee_id}/timeoff_transaction_requests`) — submit the request (this creates an approval request, not a direct transaction).

## Notes
A `*_requests` endpoint routes through ZenHR's approval workflow rather than writing the record directly. Track status via `GET /branches/{branch_id}/employees/{employee_id}/approvals`. Errors: see `errors/zenhr-inc-problem-types.yml`.

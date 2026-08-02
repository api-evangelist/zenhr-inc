---
name: Onboard an employee into a branch
description: Create a new employee under a ZenHR branch and populate their professional and financial data.
api: openapi/zenhr-inc-openapi.yml
operations: [listBranches, createEmployee, createEmployeeProfessionalData, createEmployeeFinancialData, getEmployee]
---

# Onboard an employee into a branch

Use the ZenHR API (`https://app.zenhr.com/api/v3`) to add a new employee and set up their records.

## Auth
OAuth 2.0 Authorization Code + PKCE. Send `Authorization: Bearer <access_token>`. The app must hold the `read:employee` and write authorities granted at registration. Respect rate limits (15 req / 4s production).

## Steps
1. `listBranches` (`GET /branches`) — find the target `branch_id`.
2. `createEmployee` (`POST /branches/{branch_id}/employees`) — create the employee master record; capture the returned `employee_id`.
3. `createEmployeeProfessionalData` (`POST /branches/{branch_id}/employees/{employee_id}/professional_data`) — set position, department, hire details.
4. `createEmployeeFinancialData` (`POST /branches/{branch_id}/employees/{employee_id}/financial_data`) — set salary/financial setup.
5. `getEmployee` (`GET /branches/{branch_id}/employees/{employee_id}`) — verify the onboarded record.

## Errors
Standard HTTP status codes with a JSON body (401 missing/expired token, 403 insufficient scope, 404 wrong branch/employee id, 422 validation, 429 rate limit). See `errors/zenhr-inc-problem-types.yml`.

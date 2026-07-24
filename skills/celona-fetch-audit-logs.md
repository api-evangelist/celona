---
name: Fetch Celona account audit logs
description: Retrieve and page through Celona Orchestrator account audit log records over a time window.
api: Celona API
operations:
  - GET /v1/api/audit/records
---

# Fetch Celona account audit logs

Retrieve audit log records for your Celona account over a time range.

## Auth
Send `x-api-key: <your-api-key>` and `accept: application/json`
(see `authentication/celona-authentication.yml`).

## Steps
1. `GET https://cso.celona.io/v1/api/audit/records` with query parameters:
   - `start-time` (required) — epoch milliseconds
   - `end-time` (required) — epoch milliseconds
   - `sort-columns` (optional), `sort-direction` (`asc`|`desc`, optional)
   - `page-index` (optional, 1-based), `page-size` (optional)
2. Read the response envelope: `data.total`, `data.filtered`, and
   `data.records[]`. Increment `page-index` until you have all `data.total`
   records.

## Notes
- Times are epoch milliseconds; note this endpoint uses hyphenated parameter
  names (`start-time`, `page-index`) unlike the Events API camelCase.
- Errors follow the `{ code, success, data, error }` envelope
  (`errors/celona-problem-types.yml`).

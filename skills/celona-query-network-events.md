---
name: Query Celona network events
description: Retrieve and page through Celona private-5G network events (alerts and status changes) over a time window using the v2 Events API.
api: Celona API
operations:
  - POST /v2/api/events/query
  - GET /v2/api/events/context
---

# Query Celona network events

Use the Celona Orchestrator v2 Events API to fetch network events (alerts and
status changes) for a customer over a time range.

## Auth
Every request needs the `X-API-Key` header. Generate a key in the Orchestrator
(user icon > API Keys > GENERATE KEY); the value is shown once and cannot be
recovered later. See `authentication/celona-authentication.yml`.

## Steps
1. (Optional) `GET https://cso.celona.io/v2/api/events/context` with your
   `X-API-Key` to discover the available search contexts you can filter on.
2. `POST https://cso.celona.io/v2/api/events/query` with `Content-Type: application/json`
   and the query parameters:
   - `customer_id` (required) — your customer id
   - `startTime`, `endTime` (required) — Unix timestamps in milliseconds
   - `pageSize` (required, 1–1000), `pageIndex` (default 1)
   - `order` — `ascending` or `descending` (default `descending`)
3. Read the response envelope: `data.count` (total), `data.filtered`, and
   `data.records[]`. Increment `pageIndex` until you have retrieved `data.count`
   records.

## Conventions & errors
- Times are epoch milliseconds; pagination is page-index based
  (`conventions/celona-conventions.yml`).
- Responses use the `{ code, success, data, error }` envelope.
- `400` = bad/malformed parameters; `403` = missing/invalid `X-API-Key`;
  `500` = server error (see `errors/celona-problem-types.yml`).
- The v1 events endpoints (`/v1/api/stats/events/*`) are deprecated — use v2.

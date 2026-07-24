---
name: Subscribe to Celona network alerts
description: Create interrupt-style (push) subscriptions so Celona network alerts (e.g. MEMORYALERT, CPUALERT) are delivered to a target such as Slack, instead of polling.
api: Celona API
operations:
  - POST /v1/api/events/configure
  - POST /v1/api/events/subscription
  - DELETE /v1/api/events/subscription
---

# Subscribe to Celona network alerts

Set up push notifications for Celona network events so you are notified on state
changes instead of polling the Events API.

## Auth
Send the `X-API-Key` header on every request (see
`authentication/celona-authentication.yml`).

## Steps
1. (Optional) `POST https://api.celona.io/v1/api/events/configure` to configure
   notification behavior for your account.
2. `POST https://api.celona.io/v1/api/events/subscription` with a JSON body naming
   the event type and delivery target, e.g.:
   ```json
   { "event_type": "MEMORYALERT", "notify_type": "slack" }
   ```
3. To stop notifications, `DELETE https://api.celona.io/v1/api/events/subscription`
   for that subscription.

## Event types
Valid `event_type` values include `APSTATUS`, `DEVICESTATUS`, `UEALERT`,
`DPALERT`, `CPUALERT` (>70% CPU), `MEMORYALERT` (>80% memory), `EDGESTATUS`,
`PTPSTATE`, `UDPFDHCPTIMEOUT`, `NHSTATUS`, `SITEAVAILABILITYALERT`,
`RECURRINGPODCRASHALERT`. See `asyncapi/celona-events-webhooks.yml` for the full
catalog and severities.

## Notes
- `dpalert` and `ptpstate` are discontinued for the Events v1 API in release 2025.1.
- Errors follow the `{ code, success, data, error }` envelope
  (`errors/celona-problem-types.yml`).

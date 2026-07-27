---
name: Request and schedule an M1 data extract
description: Use the M1 Gateway's pull-based bulk delivery surface - request an extract, poll for it, download the file, and put a recurring schedule in place.
api: openapi/nar-m1-gateway-external-openapi.json
generated: '2026-07-26'
method: generated
source: openapi/nar-m1-gateway-external-openapi.json
operations:
  - POST /ext/DataExtractRequest
  - GET /ext/DataExtractRequest/Requester
  - GET /ext/DataExtractRequest/FileLocation
  - GET /ext/DataExtractRequest/DownloadFile/{id}
  - GET /ext/DataExtractSchedule/AssociationId/{associationId}
  - POST /ext/DataExtractSchedule
  - PUT /ext/DataExtractSchedule/{scheduleId}
  - DELETE /ext/DataExtractSchedule/{id}
---

# Request and schedule an M1 data extract

M1 has **no webhooks and no event stream** — bulk and change delivery is pull-based through the data
extract surface. Method + path identify each operation; the published document declares no `operationId`s.

## One-off extract

1. **Request it** — `POST /ext/DataExtractRequest` with the `Data_Extract_Request` body. The response is
   the created `Data_Extract_Request`.
2. **Poll your requests** — `GET /ext/DataExtractRequest/Requester` returns the array of
   `Data_Extract_Request` records for the calling requester. Poll until the request you created reports
   as ready.
3. **Resolve the location** — `GET /ext/DataExtractRequest/FileLocation` returns a string path.
4. **Download** — `GET /ext/DataExtractRequest/DownloadFile/{id}` returns the file itself (no JSON schema
   is declared for this response).

## Recurring extracts

- **List** — `GET /ext/DataExtractSchedule/AssociationId/{associationId}` returns the
  `Data_Extract_Schedule` array for an association.
- **Create** — `POST /ext/DataExtractSchedule` (`Data_Extract_Schedule`).
- **Replace** — `PUT /ext/DataExtractSchedule/{scheduleId}` with the full schedule body.
- **Delete** — `DELETE /ext/DataExtractSchedule/{id}`.

## Rules

- Poll politely. The gateway advertises a live budget in `X-Rate-Limit-Limit` / `-Remaining` / `-Reset`
  (observed ~6,000 requests per 600s window) but documents no policy and declares no `429`; back off when
  `X-Rate-Limit-Remaining` falls.
- Extract payloads are membership PII. Treat downloaded files as regulated data.
- `PUT` on a schedule is a full replace, not a merge — read the schedule first.
- There is no idempotency key: a retried `POST /ext/DataExtractRequest` queues a second extract.

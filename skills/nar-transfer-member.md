---
name: Transfer or convert an M1 member between associations and offices
description: Move a REALTOR member to a new association or office, manage secondary affiliations, and convert an Institute Affiliate to another member type.
api: openapi/nar-m1-gateway-external-openapi.json
generated: '2026-07-26'
method: generated
source: collections/nar-m1-gateway-external.postman_collection.json
operations:
  - GET /ext/Member/Prime/{memberId}
  - POST /ext/Member/Search
  - POST /ext/Member/Transfer/{memberId}
  - POST /ext/Member/ConvertIA/{memberId}
  - GET /ext/MemberSecondary/MemberId/{memberId}
  - POST /ext/MemberSecondary
  - PATCH /ext/MemberSecondary
---

# Transfer or convert an M1 member

Method + path identify each step; NAR's Swagger 2.0 document declares no `operationId`s.

## Find the member

- `POST /ext/Member/Search` with the `BizMemberSearch` body. At least one search field is required, and
  **name fields (`FirstName`, `LastName`) cannot be the only criteria**.
- `GET /ext/Member/Prime/{memberId}` returns the prime (master) record — the authoritative state of the
  member across all associations (`Member_Master_Record`). Read this before any transfer.

## Transfer the primary affiliation

1. `POST /ext/Member/Transfer/{memberId}` with the `TransferMember` body. The member's primary
   affiliation is updated to the target association and office; the response is the updated `BizMember`.
2. Confirm with `GET /ext/Member/Prime/{memberId}`.

## Convert an Institute Affiliate

`POST /ext/Member/ConvertIA/{memberId}` with the `ConvertIAMember` body converts an IA member to member
type `R`, `RA` or `I` — used when an IA upgrades to REALTOR status.

## Secondary affiliations

- `GET /ext/MemberSecondary/MemberId/{memberId}` lists secondary association memberships.
- `POST /ext/MemberSecondary` adds one (`Member_Secondary`).
- `PATCH /ext/MemberSecondary` edits one using the test/replace pattern (see the update-member skill).

## Rules

- Transfers and conversions are single-shot POSTs with **no idempotency key**. On a timeout, re-read the
  prime record before retrying — a blind retry can move the member twice.
- `403` means your credentials do not cover the source or target association.

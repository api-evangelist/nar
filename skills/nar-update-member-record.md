---
name: Update an M1 member record with the test/replace JSON Patch pattern
description: Safely change fields on a REALTOR member or a member sub-record using NAR's required GET-then-test/replace JSON Patch flow.
api: openapi/nar-m1-gateway-external-openapi.json
generated: '2026-07-26'
method: generated
source: collections/nar-m1-gateway-external.postman_collection.json
operations:
  - GET /ext/Member/{memberId}
  - GET /ext/Member/Full/{memberId}
  - PATCH /ext/Member
  - PATCH /ext/MemberAddress
  - PATCH /ext/MemberPhone
  - PATCH /ext/MemberEmail
  - PATCH /ext/Member/Reinstate
---

# Update an M1 member record (test/replace JSON Patch)

Every `PATCH` on the M1 Gateway uses NAR's **test/replace** pattern. The document declares no
`operationId`s; steps are identified by method + path from
`openapi/nar-m1-gateway-external-openapi.json`. Media type: `application/json-patch+json`.

## Steps

1. **Read the current record first** — `GET /ext/Member/{memberId}` for core fields, or
   `GET /ext/Member/Full/{memberId}` when the target field lives on a sub-record.
2. **Build the `Updates` array** as `test` + `replace` pairs. The `test` value **must match the currently
   stored value exactly**; a mismatch returns `400`.
3. **Send the patch** to the resource endpoint — `PATCH /ext/Member` (body `MemberPatch`), or the
   sub-record equivalent: `PATCH /ext/MemberAddress`, `PATCH /ext/MemberPhone`,
   `PATCH /ext/MemberEmail`, `PATCH /ext/MemberCertification`, `PATCH /ext/MemberCoe`,
   `PATCH /ext/MemberDemographic`, `PATCH /ext/MemberDesignation`, `PATCH /ext/MemberEducation`,
   `PATCH /ext/MemberFairHousing`, `PATCH /ext/MemberFieldOfBusiness`,
   `PATCH /ext/MemberIscAffiliation`, `PATCH /ext/MemberMLS`, `PATCH /ext/MemberSecondary`.
   Identifiers travel **in the body**, not the URL (e.g. `MemberId` + `AddressTypeCode` for an address).
4. **Re-read and confirm** with the matching `GET`.

## Reinstating a member

`PATCH /ext/Member/Reinstate` (body `ReinstatePatch`) moves a member from status `I`, `T` or `S` to `A` or
`P`. NAR notes this endpoint differs from the plain member patch — it uses the reinstate-specific body.

## Rules

- `null` and `""` are **not interchangeable**; carry the stored form through the `test` value.
- A retried patch that already applied will fail its `test` and return `400`. That is the intended
  optimistic-concurrency guard — treat `400` after a timeout as "already applied", re-`GET` and compare
  before doing anything else. There is no `Idempotency-Key` header.
- `403` means the record is outside your association's permission scope, not that the patch was malformed.

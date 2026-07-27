---
name: Manage offices and associations in M1
description: Search, read, create, patch and transfer office records in the REALTORS M1 Gateway, plus secondary office affiliations and association lookups.
api: openapi/nar-m1-gateway-external-openapi.json
generated: '2026-07-26'
method: generated
source: collections/nar-m1-gateway-external.postman_collection.json
operations:
  - POST /ext/Office/Search
  - GET /ext/Office/{officeId}
  - GET /ext/Office/Full/{officeId}
  - POST /ext/Office
  - PATCH /ext/Office
  - POST /ext/Office/Transfer/{officeId}
  - GET /ext/OfficeSecondary/OfficeId/{officeId}
  - POST /ext/OfficeSecondary
  - PATCH /ext/OfficeSecondary
  - GET /ext/Association/{id}
  - POST /ext/Association/Search
  - PATCH /ext/Association
---

# Manage offices and associations in M1

Method + path identify each operation; NAR's Swagger 2.0 document declares no `operationId`s.

## Offices

1. **Find** — `POST /ext/Office/Search` with the `BizOfficeSearch` body.
2. **Read** — `GET /ext/Office/{officeId}` for the core record, `GET /ext/Office/Full/{officeId}` for the
   record with its sub-records.
3. **Create** — `POST /ext/Office` with the `Office` body.
4. **Update** — `PATCH /ext/Office` with the `OfficePatch` test/replace body (GET first; the `test` value
   must match the stored value).
5. **Transfer** — `POST /ext/Office/Transfer/{officeId}` with `TransferOffice`.

## Secondary office affiliations

`GET /ext/OfficeSecondary/OfficeId/{officeId}`, `POST /ext/OfficeSecondary`,
`PATCH /ext/OfficeSecondary` (`OfficeSecondaryPatch`).

## Associations

- `POST /ext/Association/Search` (`BizAssociationSearch`) to find an association.
- `GET /ext/Association/{id}` for the `Association` record.
- `PATCH /ext/Association` (`AssociationPatch`) using the same test/replace rules.

## Rules

- `403` on an office or association means your credentials do not cover it — the M1 permission model is
  association-scoped.
- Offices carry members: transfer the office only after confirming the downstream member impact with
  `GET /ext/Office/Full/{officeId}`.
- No pagination exists on any list endpoint; searches return the full match set.

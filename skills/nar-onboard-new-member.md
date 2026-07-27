---
name: Onboard a new REALTOR member in M1
description: Create a new member record in the REALTORS M1 Gateway, checking for duplicates first and then attaching the address, phone and email sub-records.
api: openapi/nar-m1-gateway-external-openapi.json
generated: '2026-07-26'
method: generated
source: openapi/nar-m1-gateway-external-openapi.json + collections/nar-m1-gateway-external.postman_collection.json
operations:
  - GET /ext/Member/CheckDuplicateMember/{memberId}
  - POST /ext/Member/CheckDuplicateEmail
  - POST /ext/Member/ValidateReLicense
  - POST /ext/Member/Full/{memberId}
  - POST /ext/MemberAddress
  - POST /ext/MemberPhone
  - POST /ext/MemberEmail
  - GET /ext/Member/Full/{memberId}
---

# Onboard a new REALTOR member in M1

NAR's published Swagger 2.0 document declares **no `operationId`s**, so every step below is identified by
its verified HTTP method and path exactly as they appear in
`openapi/nar-m1-gateway-external-openapi.json`. Do not invent operation names.

## Before you start

- Authenticate with **HTTP Basic** (`Authorization: Basic <base64(user:password)>`). NAR issues
  environment-specific credentials; never send test credentials to production. Base URL:
  `https://m1gateway.realtor`.
- These are membership PII writes. Creates are **not retry-safe** — there is no idempotency key.
- Response codes: `400` (check `FieldValidationErrors`), `401` (not authorized for the service), `403`
  (not permitted for this record), `404`, `500`. See `errors/nar-error-codes.yml`.

## Steps

1. **Check the member does not already exist** — `GET /ext/Member/CheckDuplicateMember/{memberId}`.
   Returns the field-validation shape (`BizValidationFieldError`). Stop if a duplicate is reported.
2. **Check the email is not already in use** — `POST /ext/Member/CheckDuplicateEmail` with the
   `BizEmailValidation` body. Stop and reconcile if the address is taken.
3. **Validate the real estate license** — `POST /ext/Member/ValidateReLicense` with the
   `BizValidationRealEstateLicense` body (license number plus state).
4. **Create the member** — `POST /ext/Member/Full/{memberId}` with the `BizMember` body. The `MemberId`
   **must begin with the local association's ID prefix**. Core data plus optional sub-records can be sent
   in one call.
5. **Attach an address** — `POST /ext/MemberAddress` with the `Member_Address` body.
   `AddressTypeCode` is required: `H` (home) or `M` (mailing); one address per type.
6. **Attach a phone** — `POST /ext/MemberPhone` (`Member_Phone`).
7. **Attach an email** — `POST /ext/MemberEmail` (`Member_Email`).
8. **Verify** — `GET /ext/Member/Full/{memberId}` and confirm the sub-records landed.

## Rules

- Preserve `null` versus `""` — NAR states they are **not interchangeable**.
- If step 4 fails after a network timeout, re-run step 1 before retrying; a blind retry can create a
  duplicate member.
- Sub-record creates are keyed by composite fields (e.g. `MemberId` + `AddressTypeCode`); re-posting the
  same key is a conflict, not an update — use the PATCH skill instead.

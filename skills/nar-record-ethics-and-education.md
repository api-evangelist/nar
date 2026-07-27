---
name: Record Code of Ethics, education and designation compliance in M1
description: Post and correct the compliance sub-records NAR tracks per member - Code of Ethics cycles, education, designations, certifications and fair housing.
api: openapi/nar-m1-gateway-external-openapi.json
generated: '2026-07-26'
method: generated
source: collections/nar-m1-gateway-external.postman_collection.json
operations:
  - GET /ext/MemberCoe/MemberId/{memberId}
  - POST /ext/MemberCoe
  - PATCH /ext/MemberCoe
  - DELETE /ext/MemberCoe
  - GET /ext/MemberEducation/MemberId/{memberId}
  - POST /ext/MemberEducation
  - GET /ext/MemberDesignation/MemberId/{memberId}
  - POST /ext/MemberDesignation
  - GET /ext/MemberCertification/MemberId/{memberId}
  - POST /ext/MemberCertification
  - GET /ext/MemberFairHousing/MemberId/{memberId}
  - POST /ext/MemberFairHousing
---

# Record ethics, education and designation compliance in M1

These are the sub-records associations are audited on. Method + path identify each operation; the
published Swagger 2.0 document declares no `operationId`s.

## Code of Ethics

- Read: `GET /ext/MemberCoe/MemberId/{memberId}` — returns cycle information, completion dates and CE
  hours.
- Create: `POST /ext/MemberCoe` (`Member_Coe`). Course codes are `COEC` (continuing education) or `COEN`
  (new member orientation). The record is keyed by `MemberId` + `CourseCode` + `CourseNumber`.
- Correct: `PATCH /ext/MemberCoe` with test/replace pairs on the patchable fields.
- Remove: `DELETE /ext/MemberCoe` — **key fields go in the request body**, not the URL. `404` if absent.

## Education and education level

- `GET /ext/MemberEducation/MemberId/{memberId}` / `POST /ext/MemberEducation` /
  `PATCH /ext/MemberEducation` / `DELETE /ext/MemberEducation`.
- `GET /ext/MemberEducationLevel/MemberId/{memberId}` / `POST /ext/MemberEducationLevel` /
  `DELETE /ext/MemberEducationLevel`.

## Designations and certifications

- `GET /ext/MemberDesignation/MemberId/{memberId}` returns `BizMemberDesignation` records;
  `POST /ext/MemberDesignation` accepts an **array** of them; `PATCH /ext/MemberDesignation` edits one.
- `GET /ext/MemberCertification/MemberId/{memberId}` / `POST /ext/MemberCertification` (e.g. CIPS, AHWD,
  ABR — requires `CertificationCode`, `CourseNumber`, `SponsoringEntityId`) /
  `PATCH /ext/MemberCertification` / `DELETE /ext/MemberCertification` (key fields in the body).

## Fair housing

`GET /ext/MemberFairHousing/MemberId/{memberId}`, `POST /ext/MemberFairHousing`,
`PATCH /ext/MemberFairHousing`, `DELETE /ext/MemberFairHousing`.

## Rules

- Every sub-record is keyed by a **composite** of `MemberId` plus code/course/date fields — read the
  existing set before posting to avoid a duplicate-key `400`.
- DELETEs send keys in the body; an agent that puts them in the URL will get `404`.
- These records drive compliance reporting. Do not post speculative or inferred completions.

# National Association of REALTORS (nar)

The National Association of REALTORS (NAR) is the largest trade association in the United States, representing roughly one million members across residential and commercial real estate. NAR is the industry body that sits above the roughly 500 local Multiple Listing Services, and it is the reason the US residential market has a machine-readable contract at all: MLS Policy Statement 7.90 requires MLS organizations owned and operated by associations of REALTORS to implement the RESO Data Dictionary and the RESO Web API and to stay current within one year of each ratification, with compliance demonstrated through the RESO Certification Process. NAR mandates that standard rather than operating it — RESO is a separate organization, and the listing data behind every certified endpoint is licensed through MLS membership, an IDX or VOW agreement, a broker relationship, or a reseller. NAR itself holds no RESO certification and publishes no self-serve developer portal. Its own API surface is REALTORS M1, the members-first engagement system that replaces NRDS: the M1 Gateway External API is a live, HTTP-Basic-authenticated REST API over member, office, association and data-extract records, whose Swagger 2.0 definition and Postman collection NAR publishes openly on GitHub for external association management system vendors and NAR partners, but whose credentials NAR issues only under a partner relationship.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/nar/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/nar/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United States
- Industry Body
- MLS
- RESO
- Standards
- Membership
- Property Listings
- IDX
- PropTech

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### REALTORS M1 Gateway External API

The external surface of NAR's REALTORS M1 Gateway — the members-first engagement system that is the single source of truth for REALTOR member data across state and local associations. The published Swagger 2.0 definition documents 64 paths under `/ext/` covering Association, Member, Member Address, Certification, Code of Ethics, Demographic, Designation, Dues Payment, Education, Email, Fair Housing, Field of Business, ISC Affiliation, Language, MLS, Military Service, Phone, Secondary and Single-Owned MLS records, plus Office, Office Secondary, Data Extract Request and Data Extract Schedule. All calls require HTTP Basic authentication with environment-specific credentials issued by NAR; PATCH operations use a test/replace JSON Patch pattern where the test value must match the current stored value. The definition and a Postman collection are published openly by NAR on GitHub for external AMS vendors and NAR partners; the API host itself answers 401 without credentials and enforces a documented rate limit.

- **Human URL:** [https://github.com/NationalAssociationOfRealtors/M1-Developer-Guide-Supplemental-Docs](https://github.com/NationalAssociationOfRealtors/M1-Developer-Guide-Supplemental-Docs)
- **Base URL:** `https://m1gateway.realtor`

#### Tags

- Real Estate
- Membership
- Associations
- Members
- Offices
- United States

#### Properties

- [OpenAPI](openapi/nar-m1-gateway-external-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/nar-m1-gateway-external.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Documentation](https://github.com/NationalAssociationOfRealtors/M1-Developer-Guide-Supplemental-Docs)
- [Documentation](https://www.nar.realtor/ae/manage-your-association/realtors-m1-members-first-engagement-system)

## RESO Posture

NAR is a **mandator, not a certificand**. MLS Policy Statement 7.90 requires association-owned MLSs to implement the RESO Data Dictionary (by January 1, 2016) and the RESO Web API (by June 30, 2016), and to keep current within one year of each ratification, with compliance demonstrated through the RESO Certification Process. NAR holds **no RESO certification of its own** — it operates no MLS, and it does not appear in the public RESO certificate directory at [reso.org/certificates](https://www.reso.org/certificates/).

The RESO specifications are freely readable ([dd.reso.org](https://dd.reso.org/), [transport.reso.org](https://transport.reso.org/)) and the certificate directory is public. The **data** behind a certified endpoint is not: access runs through MLS membership, an IDX or VOW agreement, a broker or agent sponsorship, or a reseller such as Bridge, Trestle, MLS Grid or Spark.

## Access Gate

**partner-only.** There is no self-serve sign-up on any NAR host. M1 Gateway credentials are supplied directly by NAR to external association management system vendors and NAR partners. REALTORS Property Resource (narrpr.com) is gated behind NAR membership, and its `api.` and `developer.` subdomains serve a wildcard holding page rather than a developer portal. No open data API is published.

## Common Properties

- [Website](https://www.nar.realtor/)
- [GitHub Organization](https://github.com/NationalAssociationOfRealtors)
- [LinkedIn](https://www.linkedin.com/company/national-association-of-realtors)
- [Blog](https://www.nar.realtor/blogs)
- [Newsroom](https://www.nar.realtor/newsroom)
- [Policy — MLS Policy Statement 7.90 (RETS/RESO)](https://www.nar.realtor/handbook-on-multiple-listing-policy/operational-issues-section-12-real-estate-transaction-standards-rets-policy-statement-790)
- [Policy — RETS Web API](https://www.nar.realtor/about-nar/policies/mls-policy/real-estate-transaction-standards-rets-web-api)
- [Documentation — Real Estate Transaction Standards (RETS)](https://www.nar.realtor/real-estate-transaction-standards-rets)
- [Research and Statistics](https://www.nar.realtor/research-and-statistics)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com

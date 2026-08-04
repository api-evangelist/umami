# Umami (umami)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Umami is an open source, privacy-first web analytics platform that provides website traffic insights without cookies or personal data collection, serving as a simple and fast alternative to Google Analytics. The Umami API provides full programmatic access to analytics data, website management, session tracking, event data, and team collaboration features for both self-hosted and cloud instances.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/umami/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/umami/refs/heads/main/apis.yml)

## Scope

- **Type:** Index

## Tags

- Cookieless Tracking
- Open Source
- Privacy
- Web Analytics
- Website Analytics

## Timestamps

- **Created:** 2026-03-26
- **Modified:** 2026-05-19

## APIs

### Umami API

The Umami API provides programmatic access to website analytics data including pageviews, sessions, events, and metrics, allowing developers to collect tracking data and retrieve analytics reports. Self-hosted instances use JWT bearer tokens; Umami Cloud uses API key authentication.

- **Human URL:** [https://umami.is](https://umami.is)
- **Base URL:** `https://api.umami.is`

#### Tags

- Open Source
- Privacy
- Tracking API
- Web Analytics

#### Properties

- [Documentation](https://umami.is/docs)
- [Getting Started](https://umami.is/docs/getting-started)
- [A P I Documentation](https://umami.is/docs/api)
- [Git Hub Org](https://github.com/umami-software/umami)
- [OpenAPI](openapi/umami-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/umami.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/umami.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [JSON Schema](json-schema/umami-website-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-website-list-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-website-request-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-website-stats-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-pageview-data-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-metric-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-active-visitors-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-session-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-session-list-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-session-stats-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-user-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-user-request-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-team-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-team-list-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-team-member-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-team-request-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-login-request-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-login-response-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](json-schema/umami-ok-response-schema.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Structure](json-structure/umami-website-structure.json)
- [JSON Structure](json-structure/umami-website-list-structure.json)
- [JSON Structure](json-structure/umami-website-stats-structure.json)
- [JSON Structure](json-structure/umami-session-structure.json)
- [JSON Structure](json-structure/umami-session-list-structure.json)
- [JSON Structure](json-structure/umami-metric-structure.json)
- [JSON Structure](json-structure/umami-user-structure.json)
- [JSON Structure](json-structure/umami-team-structure.json)
- [JSON Structure](json-structure/umami-team-member-structure.json)
- [J S O N L D Context](json-ld/umami-context.jsonld)
- [Example](examples/umami-website-example.json)
- [Example](examples/umami-website-stats-example.json)
- [Example](examples/umami-pageview-data-example.json)
- [Example](examples/umami-metric-example.json)
- [Example](examples/umami-session-example.json)
- [Example](examples/umami-session-stats-example.json)
- [Example](examples/umami-user-example.json)
- [Example](examples/umami-team-example.json)
- [Example](examples/umami-active-visitors-example.json)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/umami-software)
- [Website](https://umami.is)
- [Documentation](https://umami.is/docs)
- [Blog](https://umami.is/blog)
- [Pricing](https://umami.is/pricing)
- [Git Hub](https://github.com/umami-software/umami)
- [Login](https://cloud.umami.is)
- [Sign Up](https://cloud.umami.is)
- [Self Hosting](https://umami.is/docs/install)
- [Support](https://umami.is/docs/support)
- [Spectral Rules](rules/umami-spectral-rules.yml)
- [Vocabulary](vocabulary/umami-vocabulary.yaml)
- [Features](undefined)
- [Use Cases](undefined)
- [Integrations](undefined)
- [Solutions](undefined)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com

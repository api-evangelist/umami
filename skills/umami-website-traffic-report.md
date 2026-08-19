---
name: umami-website-traffic-report
description: Pull a complete traffic report for one Umami-tracked website over a date range — headline stats, a bucketed pageview series, and the top dimension breakdowns — using only documented Umami operations.
api: umami
generated: '2026-08-13'
method: generated
source: openapi/umami-websites-api-openapi.yml + openapi/umami-website-statistics-api-openapi.yml + https://docs.umami.is/docs/api/website-stats
operations:
  - listWebsites
  - getWebsite
  - getWebsiteStats
  - getWebsitePageviews
  - getWebsiteMetrics
---

# Umami website traffic report

Produces the numbers a stakeholder actually asks for: how much traffic, trending
which way, and where it came from.

## Before you start

Pick the right base URL and credential — they are not interchangeable.

| Deployment | Base | Credential |
|---|---|---|
| Umami Cloud | `https://api.umami.is/v1` (no `/api` prefix) | API key, `Authorization: Bearer <key>` |
| Self-hosted | `http://<instance>/api` | JWT from `POST /api/auth/login` |

On Cloud, API access requires the Pro plan or above. Region can be pinned with
`https://api.umami.is/v1/us` or `/v1/eu`.

`startAt` and `endAt` are **UNIX timestamps in milliseconds**, not seconds.

## Steps

### 1. Resolve the website ID — `listWebsites`

`GET /websites` with `search=<name or domain>`. The response is the standard list
envelope: `{ data: [...], count, page, pageSize }`. Take `data[0].id`.

Pass `includeTeams=true` if the site may be owned by a team rather than the
caller. If you already hold the UUID, use `getWebsite` (`GET /websites/{websiteId}`)
to confirm it resolves before spending the rest of your rate budget.

### 2. Bound the window — optional but cheap

`GET /websites/{websiteId}/daterange` returns the earliest and latest dates with
collected data. Clamp the requested range to it rather than querying a window
that predates the site's first pageview.

### 3. Headline stats — `getWebsiteStats`

`GET /websites/{websiteId}/stats?startAt=...&endAt=...`

Returns `pageviews`, `visitors`, `visits`, `bounces`, `totaltime`, and a
`comparison` object carrying the prior period when requested. Derive bounce rate
as `bounces / visits` and average visit duration as `totaltime / visits` — Umami
returns the raw components, not the ratios.

This response shape changed on 2025-10-07. If you see `uniques` or `change`
instead of `visitors` and `comparison`, you are talking to a pre-v3 self-hosted
instance.

### 4. Time series — `getWebsitePageviews`

`GET /websites/{websiteId}/pageviews?startAt=...&endAt=...&unit=day`

Returns `{ pageviews: [{x, y}], sessions: [{x, y}] }`.

**Check the granularity you got back.** Umami silently promotes `unit` to the
next largest bucket when the range exceeds its ceiling: `minute` caps at 60
minutes, `hour` at 30 days, `day` at 6 months; `month` and `year` are uncapped.
Asking for `hour` across a quarter returns daily buckets with no error and no
warning.

All timestamps are UTC. There is no timezone parameter — it was removed on
2025-11-13.

### 5. Dimension breakdowns — `getWebsiteMetrics`

`GET /websites/{websiteId}/metrics?startAt=...&endAt=...&type=<dimension>`

Call once per dimension you need. Each returns `[{x, y}]` where `x` is the value
and `y` is the count. Useful `type` values: `path`, `referrer`, `title`,
`browser`, `os`, `device`, `country`, `region`, `city`, `language`, `hostname`,
`event`.

`type=url` was renamed to `type=path` on 2025-10-07. So were the filter
parameters `url` → `path` and `host` → `hostname`.

Any of the filter parameters can also be passed alongside to narrow a
breakdown — e.g. `type=path&country=US` for top pages from US visitors.

## Budget

Umami Cloud allows **50 calls per 15 seconds per API key**, flat on every plan.
A six-dimension report is roughly 9 calls, so batching several sites into one run
will hit the ceiling quickly.

**No rate-limit headers are returned.** Nothing in a response tells you how much
budget is left, so pace deliberately — do not discover the limit by hitting it.

## Errors

| Status | Body `code` | Meaning |
|---|---|---|
| 400 | `bad-request` | No credential sent. Note: *missing* is 400, not 401. |
| 401 | `unauthorized` | Credential present but rejected, or the route is barred to API keys. |
| 404 | — | `websiteId` does not exist, or is not visible to this credential. Umami does not distinguish the two. |

The envelope is `{"error":{"message","code","status"}}` — plain JSON, not
RFC 9457. Branch on `code`, never on `message`.

All reads here are safe to retry. See `errors/umami-problem-types.yml`.

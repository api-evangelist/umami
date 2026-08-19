---
name: umami-onboard-a-website
description: Register a new website in Umami, retrieve its tracking ID, optionally enable a public share URL and session replay, and verify data is arriving — the full zero-to-tracking flow.
api: umami
generated: '2026-08-13'
method: generated
source: openapi/umami-websites-api-openapi.yml + https://docs.umami.is/docs/api/websites + https://docs.umami.is/docs/tracker-configuration
operations:
  - listWebsites
  - createWebsite
  - getWebsite
  - updateWebsite
  - getActiveVisitors
---

# Onboard a website into Umami

Takes a domain from "not tracked" to "reporting traffic".

## Before you start

Cloud base `https://api.umami.is/v1`, credential `Authorization: Bearer <api-key>`,
Pro plan or above. Self-hosted base `http://<instance>/api`, credential a JWT from
`POST /api/auth/login`.

## Steps

### 1. Check it isn't already registered — `listWebsites`

`GET /websites?search=<domain>&includeTeams=true`

**Do this first.** There is no uniqueness constraint on `domain` and no
idempotency key on create, so a re-run without this check produces a duplicate
website with a second ID and split analytics. Umami will not stop you.

### 2. Create it — `createWebsite`

`POST /websites`

```json
{
  "name": "Example",
  "domain": "example.com"
}
```

Optional fields:

- `teamId` — create it under a team rather than the calling user. Team ownership
  is decided here; there is no documented move-between-owners operation.
- `shareId` — a unique string that enables the public share URL immediately.
- `id` — force a specific UUID. Use this when migrating between instances so the
  tracking snippets already deployed keep working.

Returns `201` with the full website object. **`id` is the tracking ID** you need
for the next step.

On `400`, a required field is missing or malformed; the body carries the standard
`{"error":{"message","code","status"}}` envelope.

### 3. Install the tracker

Not an API call — hand this to whoever owns the site's HTML:

```html
<script defer src="https://cloud.umami.is/script.js"
        data-website-id="<the id from step 2>"></script>
```

Self-hosted instances serve the script from their own origin. Useful attributes:
`data-auto-track="false"` to disable automatic pageviews, `data-domains` to
allowlist hostnames, `data-tag` to label all events from this deployment,
`data-do-not-track` to honor the browser setting.

The script URL carries no version segment — it floats to whatever the instance
currently serves.

### 4. Optionally enable session replay and heatmaps — `updateWebsite`

`POST /websites/{websiteId}`

```json
{
  "replayConfig": {
    "replayEnabled": true,
    "heatmapEnabled": true,
    "sampleRate": 0.15,
    "heatmapSampleRate": 0.15,
    "maskLevel": "strict",
    "maxDuration": 3600,
    "blockSelector": ".sensitive"
  }
}
```

Three things to know:

- `replayConfig` became a **nested object on 2026-05-28**. `replayEnabled` used
  to be a top-level field. If you are targeting an older self-hosted instance,
  the flat form applies.
- Replay is a **Business plan** feature on Umami Cloud.
- Replay also requires the separate recorder script (`r.js`) on the page. The
  tracker alone will not record.
- Replay collects far more than pageview analytics. Input fields are masked by
  default; `maskLevel: "strict"` and `blockSelector` narrow it further. This is
  the one Umami feature where the privacy-by-default claim becomes the
  customer's responsibility rather than the product's.

Note this is a **POST, not a PATCH or PUT**, and it is a partial update — send
only the fields you intend to change.

### 5. Optionally enable a share URL — `updateWebsite`

`POST /websites/{websiteId}` with `{"shareId": "some-unique-string"}`.

The dashboard then renders at `https://<instance>/share/<shareId>/<name>` to
**anyone holding the link, with no credential at all**. Set `shareId` to `null`
to revoke. Treat the share ID as a bearer secret.

To embed it, the instance must also allow your origin via the
`ALLOWED_FRAME_URLS` environment variable.

### 6. Verify data is arriving — `getActiveVisitors`

`GET /websites/{websiteId}/active` returns `{"visitors": <n>}`.

Load the tracked page yourself, then poll this. A non-zero count confirms the
script, the website ID and the collection endpoint all line up.

If it stays at zero: confirm the tracker is not being blocked, confirm
`data-website-id` matches exactly, and confirm the collection host. **Umami Cloud
collection moved to `gateway.umami.is` on 2026-06-06** — a stale
`data-host-url` pointing at the old host is a common cause.

## Cleanup and teardown

- `POST /websites/{websiteId}/reset` — wipes all collected data, keeps the
  website and its ID. Returns `{"ok": true}`.
- `DELETE /websites/{websiteId}` — removes the website and its data. Returns
  `{"ok": true}`.

Both are irreversible and neither takes a confirmation parameter. There is no
undo and no idempotency key.

## Budget

50 calls per 15 seconds per Cloud API key. No rate-limit headers are returned.

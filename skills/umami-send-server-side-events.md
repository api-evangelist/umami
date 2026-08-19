---
name: umami-send-server-side-events
description: Record pageviews and custom events into Umami from a server, worker or agent using the unauthenticated collection endpoints — including the retry and duplicate-billing hazards that have no guardrail.
api: umami
generated: '2026-08-13'
method: generated
source: https://docs.umami.is/docs/api/sending-stats + https://docs.umami.is/docs/guides/send-server-side-events + https://docs.umami.is/docs/api/node-client
operations:
  - sendEvent
  - sendBatch
---

# Send events to Umami server-side

The collection surface is separate from the analytics API: different host,
different rules, **no credential at all**.

## Endpoints

| Endpoint | Purpose |
|---|---|
| `POST /api/send` | One event. |
| `POST /api/batch` | A JSON **array** of `/api/send` bodies. |

Host:

- **Umami Cloud** — `https://gateway.umami.is` as of 2026-06-06. Previously
  `https://cloud.umami.is/api/send`.
- **Self-hosted** — `http://<instance>/api/send`.

These operations are not present in `openapi/`, which captures the authenticated
analytics surface only. They are documented at
<https://docs.umami.is/docs/api/sending-stats>.

## Two non-obvious requirements

1. **No authentication token.** Do not send one. Possession of the website ID is
   the entire grant, which also means anyone who reads your page source can post
   events into your account.
2. **A valid `User-Agent` header is mandatory.** Umami parses it for browser, OS
   and device attribution. A request without one is **silently not registered** —
   you get no error, and the event simply never appears. This is the single most
   common server-side integration failure.

## Payload

```json
{
  "payload": {
    "website": "your-website-id",
    "hostname": "example.com",
    "url": "/checkout",
    "title": "Checkout",
    "referrer": "https://google.com",
    "language": "en-US",
    "screen": "1920x1080",
    "tag": "backend",
    "id": "session-identifier",
    "name": "purchase",
    "data": { "plan": "pro", "revenue": "49.00" }
  },
  "type": "event"
}
```

`type` is one of `event`, `identify` or `performance`.

Response: `{"cache": "...", "sessionId": "...", "visitId": "..."}`. Echo the
`cache` value back on subsequent requests in the same session to keep Umami's
session attribution coherent.

## Batching

`POST /api/batch` takes a JSON **array** of the objects above. Each element is
forwarded to `/api/send`, so every `type` and field works identically.

```json
{ "size": 2, "processed": 2, "errors": 0, "details": [], "cache": "..." }
```

**The batch endpoint returns HTTP 200 even when items fail.** `errors` is the
failure count and `details[]` lists each failure with its `index` in the array
you submitted. A client that checks only the status code loses events without
noticing. Always read `errors`, and re-submit only the indices named in
`details[]`.

## The retry hazard — read this before writing a retry loop

Umami documents **no idempotency key** on `/api/send` or `/api/batch`. There is
no `Idempotency-Key` header, no client-supplied dedupe token, and no
server-side request deduplication.

Consequences:

- A request that times out after the server accepted it, then gets retried,
  records the event **twice**.
- Events are Umami's **billing unit** — and so is every stored `data` property.
  Duplicated events inflate both your analytics and your invoice.
- Overage is billed automatically without interrupting collection
  ($0.00003/event on Pro, $0.00002/event on Business), so a runaway retry loop
  does not fail loudly. It just costs money.

Recommended handling:

- Do **not** blind-retry on timeout. Treat an ambiguous outcome as sent.
- Retry only on a connection error where you can be confident nothing was
  written, and on 5xx.
- Prefer `/api/batch` with a bounded array and a single attempt over per-event
  retries.
- Keep `payload.data` lean. Each stored property is billed like a pageview, so a
  twelve-key metadata blob turns one event into thirteen.

## Simpler path

`@umami/node` wraps this:

```js
import umami from '@umami/node';

umami.init({ websiteId: '...', hostUrl: 'https://cloud.umami.is' });
umami.track({ url: '/checkout', name: 'purchase', data: { plan: 'pro' } });
```

Caveat: `@umami/node` was last published **0.4.0 on 2024-08-19** and predates the
2026-06-06 collection-host move, so verify what `hostUrl` it actually posts to
before relying on it.

## Identifying users

`type: "identify"` attaches a `distinctId` and/or session properties to the
session. This is the point where Umami stops being anonymous by default —
Umami's own security page places responsibility for that choice on the customer.
Do not pass an email address or any other direct identifier unless your privacy
policy covers it.

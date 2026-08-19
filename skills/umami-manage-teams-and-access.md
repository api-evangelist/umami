---
name: umami-manage-teams-and-access
description: Create Umami teams, add members via the access code, assign websites to a team, and revoke access — plus which of these operations Umami Cloud refuses to an API key.
api: umami
generated: '2026-08-13'
method: generated
source: openapi/umami-teams-api-openapi.yml + openapi/umami-users-api-openapi.yml + https://docs.umami.is/docs/api/teams + https://docs.umami.is/docs/cloud/api-key
operations:
  - listTeams
  - createTeam
  - joinTeam
  - getTeam
  - deleteTeam
  - listTeamWebsites
  - createUser
  - getUser
  - deleteUser
  - createWebsite
---

# Manage Umami teams and access

## Read this before you plan an automation

**Umami Cloud bars API keys from the user-management routes entirely.** The
documented exclusion list is:

```
/me/password
/users
/users/*
```

So on Umami Cloud, `createUser`, `getUser` and `deleteUser` are **not
available**, no matter how valid your key is — they return `401`. User
provisioning on Cloud is dashboard-only. There is no SCIM endpoint either.

The user operations below therefore apply to **self-hosted Umami** only. Team
operations work on both.

## Team lifecycle

### 1. List teams — `listTeams`

`GET /teams` returns the standard list envelope
`{ data, count, page, pageSize }`. Accepts `page`, `pageSize`, `search`.

### 2. Create a team — `createTeam`

`POST /teams`

```json
{ "name": "Marketing" }
```

Returns `201` with the team object, including an **`accessCode`**.

### 3. The access code is a bearer secret

`accessCode` is the join token. Anyone who posts it to `POST /teams/join` becomes
a member of the team:

```json
{ "accessCode": "the-code-from-step-2" }
```

There is no invitation, no email confirmation and no approval step in this flow —
possession of the string is the grant. Treat it exactly as you would an API key:
do not log it, do not put it in a ticket, and rotate it (via
`POST /teams/{teamId}`) if it leaks.

### 4. Inspect a team — `getTeam`

`GET /teams/{teamId}` returns the team including its `members` array. Each member
carries a `role`, so authorization in Umami is **per team**, not global.

### 5. List the team's websites — `listTeamWebsites`

`GET /teams/{teamId}/websites`.

To *put* a website under a team, set `teamId` at creation time —
`POST /websites` with `{"name": "...", "domain": "...", "teamId": "..."}`. Note
that `/teams/{teamId}/websites/{websiteId}` was **deprecated on 2024-02-16** and
no replacement was named in the changelog.

### 6. Remove a team — `deleteTeam`

`DELETE /teams/{teamId}` returns `{"ok": true}`.

Irreversible, no confirmation parameter, no idempotency key. Websites owned by
the team go with it. List `listTeamWebsites` first and confirm the blast radius.

## User lifecycle — self-hosted only

- `POST /users` → `201`. Body `{ "username": "...", "password": "...", "role": "..." }`.
- `GET /users/{userId}` → the user object.
- `DELETE /users/{userId}` → `{"ok": true}`.
- `GET /users/{userId}/websites` and `GET /users/{userId}/teams` enumerate what a
  user can reach — run these before deleting anyone.

Passwords are set in plaintext over the wire (TLS only). There is no invite flow
and no password-reset API; `/me/password` is the self-service path and it too is
barred to Cloud API keys.

## Account-level controls that are not in this API

- **Two-factor authentication** exists at `/api/2fa/*` with admin enforcement at
  `/api/admin/2fa/global` and per-team at `/api/admin/teams/{teamId}/2fa`. It
  protects interactive login; it does **not** gate API-key use.
- **SAML SSO** is an Enterprise-plan feature (`/api/auth/sso`). No SAML metadata
  endpoint or configuration reference is published.
- **Audit log** is Enterprise-only and has no documented API.

## Errors

| Status | `code` | Meaning |
|---|---|---|
| 400 | `bad-request` | Missing credential, or a malformed create body. |
| 401 | `unauthorized` | Bad credential — **or a route your Cloud API key is barred from**. |
| 404 | — | Team or user does not exist, or is not visible to this credential. |

A `401` on `/users` from a key you know is good is not a bug. It is the Cloud
route restriction.

## Budget

50 calls per 15 seconds per Cloud API key. No rate-limit headers are returned;
pace yourself.

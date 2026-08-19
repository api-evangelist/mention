---
name: mention-monitor-a-brand
description: >-
  Stand up brand monitoring on Mention from nothing: resolve the calling account, create an alert for
  the brand and its competitors, confirm it is collecting, and read the first page of mentions.
api: mention
base_url: https://api.mention.net/api
operations:
  - getMe
  - getAppData
  - createAlert
  - getAlert
  - listMentions
generated: '2026-08-13'
method: generated
source: https://dev.mention.com/current/
---

# Monitor a brand on Mention

Set up a standing keyword query (an **alert**) and read what it collects.

## Before you start

- Base URL is `https://api.mention.net/api`.
- Every request carries `Authorization: Bearer <access_token>` and `Accept-Version: 1.21`.
  Without the version header you get whatever version the app's settings record.
- Write requests carry `Content-Type: application/json`.
- **Retries are not safe.** Mention documents no idempotency key. If `createAlert` times out, do not
  blind-retry — call `listAlerts` first and check whether the alert landed.

## Steps

### 1. Resolve the account — `getMe`

```
GET /accounts/me
```

Read `account.id`. It is a **composite string** like
`12345_69gjjsg4itgkcco040okwsck700o4w8gsco0k4kco0s4scw8o0`, not an integer. Every other operation
needs it in the path. Do not derive it, do not cache it across tokens.

### 2. Read the vocabularies — `getAppData`

```
GET /app/data
```

This is the only source of truth for the enumerated values you are about to send. Pull
`alert_sources` (valid `sources`), `alert_languages` (valid `languages`) and `alert_countries`.
Hard-coding these values is the most common way an integration drifts.

### 3. Create the alert — `createAlert`

```
POST /accounts/{account_id}/alerts?group_id={group_id}
```

`group_id` is **mandatory** as a query string parameter since API version 1.21. It was optional
before; a caller written against older docs will fail here.

A basic query, when you want a keyword set:

```json
{
  "name": "NASA and competitors",
  "query": {
    "type": "basic",
    "included_keywords": ["NASA", "Arianespace", "SpaceX"],
    "required_keywords": ["mars"],
    "excluded_keywords": ["nose"]
  },
  "languages": ["en"],
  "sources": ["web", "twitter"]
}
```

An advanced query, when you need boolean logic:

```json
{
  "name": "Space innovation",
  "query": {"type": "advanced", "query_string": "(space AND innovation) OR (NASA AND Discovery)"},
  "languages": ["en"]
}
```

Rules that will bite you:

- At least one of `included_keywords` or `required_keywords` must be non-empty.
- `included_keywords` is OR, `required_keywords` is AND, `excluded_keywords` is NOT. Each keyword
  matches exactly.
- A single boolean clause may not mix `AND` and `OR` — add parentheses.
- Keywords accept alphanumerics, whitespace and `._@#'&`; `-` only when preceded by an alphanumeric.
- Alert creation is rate limited to `max(20, alertsQuota * 2)` calls per 24 hours, per account.

Read `alert.id` off the response.

### 4. Confirm it is collecting — `getAlert`

```
GET /accounts/{account_id}/alerts/{alert_id}?stats=unread_mentions.total,mention_folders.inbox.total
```

Since version 1.21 the `stats` field is **empty by default**. Ask for the counters you want by name,
comma-separated. Also read `index_version` here — it decides which mention filters this alert
supports in the next step.

### 5. Read the mentions — `listMentions`

```
GET /accounts/{account_id}/alerts/{alert_id}/mentions?limit=20&folder=inbox
```

Collection results come back as `{"mentions": [...], "_links": {...}}`. Page **older** with
`_links.more.href`; poll for **newer** with `_links.pull.href`.

`listMentions` is capped at **3600 calls per alert per 24 hours** — an average of one call every 24
seconds. If you need more than that, use the streaming skill instead of polling harder.

## Failure handling

| Status | Meaning | What to do |
|---|---|---|
| 400 | Validation failure | Read `form.errors` and `form.children.<field>.errors`. These are human-readable strings, not codes. Fix the named field; do not retry unchanged. |
| 401 | Token missing or invalid | Re-read the token from the app settings page or re-run the OAuth2 flow. No refresh grant is documented. |
| 402 | Plan does not cover this | You used an entitlement-gated capability, or API access is not on the account's plan. |
| 403 | Not permitted on this resource | You are acting on another account. Get a token for that account. |
| 429 | Rate limited | Sleep until the unix timestamp in `X-Rate-Limit-Reset`, then retry. No `Retry-After` and no remaining-budget header is returned. |

Outside 200 and 400, the body is **HTML by design**. Do not parse it.

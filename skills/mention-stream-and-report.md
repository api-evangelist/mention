---
name: mention-stream-and-report
description: >-
  Keep a live feed of new mentions without burning the polling quota, then pull the aggregate
  statistics and influencer list behind a reporting period.
api: mention
base_url: https://api.mention.net/api
operations:
  - getMe
  - listAlerts
  - streamMentions
  - listMentions
  - getStats
  - listAuthors
generated: '2026-08-13'
method: generated
source: https://dev.mention.com/current/
---

# Stream mentions and build a report

Two jobs that share one constraint: `listMentions` is capped at 3600 calls per alert per 24 hours,
so real-time work belongs on the stream and reporting belongs on `getStats`.

## Before you start

- REST base URL is `https://api.mention.net/api`.
- The **streaming base URL is different**: `https://stream.mention.net/api`. `streamMentions` is not
  served from the REST host.
- `Authorization: Bearer <token>` and `Accept-Version: 1.21` on every call.
- Resolve `account_id` with `GET /accounts/me`, then `listAlerts` to get the alert ids you care
  about.

## Part 1 — live feed

### 1. Open the stream — `streamMentions`

```
GET https://stream.mention.net/api/accounts/{account_id}/mentions?alerts[]=112233&alerts[]=112234
```

The connection stays open and each mention arrives as it is collected. Constraints Mention states
explicitly:

- **One open stream at a time per account.** A second connection for the same `account_id` is not
  supported — multiplex alerts onto one stream via repeated `alerts[]` parameters instead.
- Only available on `stream.mention.net`. Calling this path on the REST host will not stream.

### 2. Reconnect and backfill

There is no documented resume token. After a disconnect, close the gap with a bounded REST read
rather than replaying the stream:

```
GET /accounts/{account_id}/alerts/{alert_id}/mentions?since_id={last_seen_id}&limit=1000
```

`since_id` returns mentions found after the given id, ordered by id — which is collection order, not
publication order. It cannot be combined with `before_date`, `not_before_date` or `cursor`.

Alternatively follow `_links.pull.href` from your last listing response, which is the idiom Mention
documents for polling forward.

Budget the backfill: every REST read counts against the 3600/alert/24h ceiling.

## Part 2 — the report

### 3. Aggregate statistics — `getStats`

```
GET /accounts/{account_id}/stats
  ?alerts[]=112233
  &from=2026-07-01T00:00:00.0
  &to=2026-07-31T23:59:59.0
  &timezone=Europe/Berlin
  &interval=P1D
  &tones_per_interval_stats=true
  &country_stats=true
  &week_day_stats=true
  &influencers=true
```

Everything is a query string parameter, and the whole query string must be **URL encoded**.

- `alerts[]` is required and repeats per alert.
- `interval` is `P1D` daily, `P1W` weekly or `P1M` monthly.
- `week_day_stats` and `week_day_by_hour_stats` **cannot coexist** in the same query.
- `favorite` and `important` can be combined with each other.
- Narrow with the array filters: `tones[]`, `languages[]`, `sources[]`, `countries[]`, `tags[]`.
- Dates use the W3C level 6 format `YYYY-MM-DDThh:mm:ss.sTZD`; the fractional second is not
  decorative — it is what makes date-based pagination deterministic elsewhere in the API.

### 4. Influencers — `listAuthors`

```
GET /accounts/{account_id}/alerts/{alert_id}/authors?kind=twitter&from=...&to=...&sort=score&order=desc&limit=25
```

Each author carries `influencer_score`, `enriched`, and a `main_author` object with the social
profile: `kind`, `url`, `name`, `realname`, `score` and `followers_count`.

Do not build on `gender` — it was deprecated as a chart dimension in API version 1.19 (2020-03-31)
and soft-deprecated for every prior version.

## Cautions

- Mention ships **no webhooks and no AsyncAPI**. The long-lived HTTP response above is the only push
  surface; there is nothing to subscribe to and nothing to register a callback with.
- On 429 the only signal is `X-Rate-Limit-Reset`, a unix timestamp. There is no `Retry-After`, no
  limit and no remaining header, so pace conservatively rather than probing for the ceiling.
- Mentions come back with at most 250 characters of content, so a report built from this API
  summarises reach and classification, not full text.

---
name: mention-triage-and-curate-mentions
description: >-
  Work an alert's inbox on Mention: page the mentions, classify them with folder, tone and tags,
  assign the ones that need a human to a team mate, and clear the rest.
api: mention
base_url: https://api.mention.net/api
operations:
  - getAppData
  - listMentions
  - getMention
  - createAlertTag
  - listAlertTags
  - curateMention
  - createMentionTask
  - updateTask
  - listAlertTasks
  - markAllMentionsRead
generated: '2026-08-13'
method: generated
source: https://dev.mention.com/current/
---

# Triage and curate an alert's mentions

Turn a raw feed into a classified, assigned, cleared inbox.

## Before you start

- Base URL `https://api.mention.net/api`; every call carries `Authorization: Bearer <token>` and
  `Accept-Version: 1.21`.
- Resolve `account_id` with `GET /accounts/me` first. It is a composite string, not an integer.
- Read `alert.index_version` off the alert. It decides which filters below are available to you.

## Steps

### 1. Load the vocabularies — `getAppData`

```
GET /app/data
```

Take `alert_tones` (valid `tone` keys), `mention_folders` (valid `folder` values) and `task_types`
(valid task `type` values). Do not hard-code them.

### 2. Page the inbox — `listMentions`

```
GET /accounts/{account_id}/alerts/{alert_id}/mentions?folder=inbox&unread=1&limit=100
```

Filter combinations that are **forbidden** and will fail:

- `unread` cannot be combined with `favorite`, `q` or `tone`.
- `favorite` cannot be combined with `folder` unless the folder is `inbox` or `archive`.
- `since_id` cannot be combined with `before_date`, `not_before_date` or `cursor`.

Filters that need entitlement — a plan with **search access** — and return 402 without it:
`source`, `sort`, `q`.

Page older results with `_links.more.href`. `limit` defaults to 20 and caps at 1000; smaller pages
respond faster.

### 3. Create the tags you will use — `createAlertTag`

```
POST /accounts/{account_id}/alerts/{alert_id}/tags
{"name": "competitor", "keywords": ["spacex", "arianespace"]}
```

A mention can only be tagged with a tag that **already exists on its alert**, so create tags before
curating. Limits: 100 tags per alert, 20 characters per name, 5 auto-tagging keywords per tag. Any
new mention containing a keyword is tagged automatically.

Call `listAlertTags` to read back the ids — curation binds by `id`, not by name, which is also why
renaming a tag later keeps every existing association.

### 4. Classify each mention — `curateMention`

```
PUT /accounts/{account_id}/alerts/{alert_id}/mentions/{mention_id}
{"read": true, "folder": "archive", "tone": 1, "tags": [{"id": 46468}]}
```

PUT is a **partial update** — send only what changes. Notes:

- `tone` is `-1` negative, `0` neutral, `1` positive.
- `folder` is one of `inbox`, `archive`, `spam`, `trash`.
- Marking `favorite: true` automatically marks the mention read.
- `favorite` and `trashed` are **admin only**.
- You cannot modify a mention's content or source — only its classification.

The response is the full mention, exactly as `getMention` would return it.

### 5. Assign what needs a human — `createMentionTask`

```
POST /accounts/{account_id}/alerts/{alert_id}/mentions/{mention_id}/tasks
{"type": "reply", "assigned_to_account_id": "{teammate_account_id}", "comment": "Deserves a reply this week."}
```

`assigned_to_account_id` is required and is **not updatable** — reassigning means deleting the task
and creating a new one. `type`, `comment` and `done` can be changed later with `updateTask`.

There is no endpoint for "the tasks of a mention" — they come back inline on the mention. To review
open work across the whole alert use:

```
GET /accounts/{account_id}/alerts/{alert_id}/tasks
```

### 6. Close out — `updateTask` and `markAllMentionsRead`

```
PUT /accounts/{account_id}/alerts/{alert_id}/mentions/{mention_id}/tasks/{task_id}
{"done": true}
```

```
POST /accounts/{account_id}/alerts/{alert_id}/mentions/markallread
```

`markAllMentionsRead` clears the whole alert at once. It is not reversible per mention in bulk, so
run it only after the pass is complete.

## Cautions

- **No idempotency.** A retried `createAlertTag` or `createMentionTask` creates a duplicate. Before
  retrying a write that timed out, list the collection and check.
- **Content is truncated.** Mention returns "only up to 250 characters of the most relevant parts of
  the mention". Do not expect full article text.
- **Rate limit.** `listMentions` is capped at 3600 calls per alert per 24 hours. Batch with a larger
  `limit` rather than looping with a small one.
- On 429, sleep until the unix timestamp in `X-Rate-Limit-Reset`. There is no `Retry-After` and no
  remaining-budget header.

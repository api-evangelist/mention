---
name: mention-share-an-alert-with-a-team
description: >-
  Give team mates access to a Mention alert, tune each person's notification settings, revoke access,
  and understand why deleting the last share deletes the alert.
api: mention
base_url: https://api.mention.net/api
operations:
  - getMe
  - getAppData
  - listAlertShares
  - createAlertShare
  - getShare
  - updateShare
  - deleteShare
  - getAlertPreferences
  - updateAlertPreferences
generated: '2026-08-13'
method: generated
source: https://dev.mention.com/current/
---

# Share an alert with a team

In Mention, a **share** is the authorization object. It is also the lifecycle owner of the alert —
there is no delete-alert operation, so understanding shares is how you avoid destroying an alert by
accident.

## Before you start

- Base URL `https://api.mention.net/api`, `Authorization: Bearer <token>`, `Accept-Version: 1.21`.
- Resolve your own `account_id` with `GET /accounts/me`.
- You need the **public account id** of each team mate you are sharing with. It is the same composite
  string form: `12345_69gjjsg4itgkcco040okwsck700o4w8gsco0k4kco0s4scw8o0`.

## Steps

### 1. See who has access — `listAlertShares`

```
GET /accounts/{account_id}/alerts/{alert_id}/shares
```

Each share carries the linked `account`, a `role`, a `permissions` map and a `weight`. When an alert
is first created, the creator's account is the only entry in `shares`.

Valid role values come from `alert_share_roles` in `GET /app/data` — read them, do not assume.

### 2. Grant access — `createAlertShare`

```
POST /accounts/{account_id}/alerts/{alert_id}/shares
{"account_id": "12345_69gjjsg4itgkcco040okwsck700o4w8gsco0k4kco0s4scw8o0"}
```

The `account_id` in the **body** is the account being granted access; the `account_id` in the
**path** is the account that owns the call. Getting those two the wrong way round is the usual
mistake here.

No idempotency key exists. If this call times out, call `listAlertShares` and check before retrying.

### 3. Block rather than delete — `updateShare`

```
PUT /accounts/{account_id}/alerts/{alert_id}/shares/{share_id}
{"blocked": true}
```

`blocked` is the only updatable property on a share, and it is the **safe** way to remove someone's
access: it revokes them without touching the alert's lifecycle. Prefer this to `deleteShare` in any
automated flow.

### 4. Revoke — `deleteShare`, and the trap

```
DELETE /accounts/{account_id}/alerts/{alert_id}/shares/{share_id}
```

Read this before you automate it:

> "When all shares on an alert are deleted, the alert itself gets deleted."

There is no `DELETE /alerts/{alert_id}`. Deleting the last remaining share is how an alert is
destroyed — including all of its collected mentions. An agent iterating over shares and deleting
them will silently destroy the alert on the final iteration.

Guard rail: before every `deleteShare`, call `listAlertShares` and refuse if the count is 1 unless
the caller explicitly asked to delete the alert.

Deleting someone else's share requires **team admin** rights; without them the call returns 403.

### 5. Tune per-person notifications — alert preferences

Preferences are per account, per alert — your settings on an alert are not your team mate's.

```
GET /accounts/{account_id}/alerts/{alert_id}/preferences
```

```json
{
  "preferences": {
    "email_notification_frequency": "default",
    "push_notification_frequency": "default",
    "desktop_notification_frequency": "default",
    "trending_email_notification_frequency": "default",
    "trending_sms_notification_frequency": "default",
    "weight": 10000
  }
}
```

```
PUT /accounts/{account_id}/alerts/{alert_id}/preferences
{"push_notification_frequency": "never", "weight": 1}
```

Any attribute returned by the GET can be updated and any may be omitted. Valid frequency values come
from the `*_notification_frequencies` maps in `GET /app/data`; Mention warns that these vocabularies
"may vary in the future without special notice", so resolve them at runtime. `weight` controls how
prominent the alert is in the interface.

## Cautions

- An access token can only act on **its own** account. Reading, updating or deleting another
  person's account returns 403 even if your app created that account.
- Shares are the only permission surface — there is no separate roles or ACL resource.
- Deleting the last share deletes the alert. Always count first.

# Mention

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Mention is a media monitoring and social listening platform that watches over one billion sources in real time across 42 languages, covering social networks, news, blogs, forums, video and 75+ review sites. Its JSON-based RESTful API, documented at dev.mention.com, gives developers programmatic access to alerts (the standing keyword queries Mention crawls for), the mentions those alerts collect, curation by folder/tone/tag, mention tasks assigned to team mates, alert sharing and permissions, authors and influencer scoring, aggregate statistics, and a long-lived streaming endpoint for real-time delivery. Authentication is a bearer token obtained from a registered app or through an OAuth2 authorization-code flow, and the API version is selected per request with an Accept-Version header. Mention is sold by Agorapulse SAS; API access is a paid add-on rather than an included capability of the published plan.

**Areas:** Alerts · Brand Monitoring · Media Monitoring · Social Listening · Social Media · Sentiment Analysis · Reputation Management · Influencer Marketing · Competitive Intelligence · Streaming · Marketing

## Mention on APIs.io

Everything profiled for Mention is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/mention/](https://apis.io/providers/mention/)** — or go straight to one of the APIs or properties below.

### APIs (9)

- [Mention Accounts API](https://apis.io/apis/mention/mention-accounts-api/) — The Accounts API from Mention — 5 operations for account CRUD and identity resolution — createAccount, getAccount, updateAccount, deleteAccount and getMe.…
- [Mention Alerts API](https://apis.io/apis/mention/mention-alerts-api/) — The Alerts API from Mention — 8 operations for the standing keyword queries Mention crawls the web and social media for — list, create, read, update, pause…
- [Mention Authors API](https://apis.io/apis/mention/mention-authors-api/) — The Authors API from Mention — 1 operation. listAuthors returns the authors and influencers behind an alert’s mentions with influence score, reach and the…
- [Mention Mentions API](https://apis.io/apis/mention/mention-mentions-api/) — The Mentions API from Mention — 6 operations over the results an alert collects — list with an extensive filter surface, read one, read its grouped…
- [Mention Shares API](https://apis.io/apis/mention/mention-shares-api/) — The Shares API from Mention — 5 operations over the join between an account and an alert. Share is the authorization primitive AND the lifecycle owner of an…
- [Mention Stats API](https://apis.io/apis/mention/mention-stats-api/) — The Stats API from Mention — 1 operation. getStats returns aggregate counters alert by alert over a date range and interval, with week-day, per-interval…
- [Mention Tags API](https://apis.io/apis/mention/mention-tags-api/) — The Tags API from Mention — 4 operations for the labels scoped to an alert, including up to five auto-tagging keywords per tag that label incoming mentions…
- [Mention Tasks API](https://apis.io/apis/mention/mention-tasks-api/) — The Tasks API from Mention — 5 operations for the assignments created on a mention for team mates. assigned_to_account_id is fixed at creation; type,…
- [Mention App API](https://apis.io/apis/mention/mention-app-api/) — The App API from Mention — 1 operation. GET /app/data returns the reference vocabularies every other Mention endpoint resolves its enumerated values…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [mention](https://apis.io/servers/mention/mention/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Mention Agentic Access](https://apis.io/agentic-access/mention/mention-agentic-access/)

#### Collections (10)

Postman and Open Collection request collections.

- [Mention API](https://apis.io/collections/mention/open-mention/)
- [Mention Accounts API](https://apis.io/collections/mention/open-mention-accounts-api/)
- [Mention Alerts API](https://apis.io/collections/mention/open-mention-alerts-api/)
- [Mention App API](https://apis.io/collections/mention/open-mention-app-api/)
- [Mention Authors API](https://apis.io/collections/mention/open-mention-authors-api/)
- [Mention Mentions API](https://apis.io/collections/mention/open-mention-mentions-api/)
- [Mention Shares API](https://apis.io/collections/mention/open-mention-shares-api/)
- [Mention Stats API](https://apis.io/collections/mention/open-mention-stats-api/)
- [Mention Tags API](https://apis.io/collections/mention/open-mention-tags-api/)
- [Mention Tasks API](https://apis.io/collections/mention/open-mention-tasks-api/)

#### Overlays (9)

OpenAPI overlays, including conversational phrasing for each operation.

- [Mention Accounts Overlay](https://apis.io/overlays/mention/mention-accounts-overlay/)
- [Mention Alerts Overlay](https://apis.io/overlays/mention/mention-alerts-overlay/)
- [Mention App Overlay](https://apis.io/overlays/mention/mention-app-overlay/)
- [Mention Authors Overlay](https://apis.io/overlays/mention/mention-authors-overlay/)
- [Mention Mentions Overlay](https://apis.io/overlays/mention/mention-mentions-overlay/)
- [Mention Shares Overlay](https://apis.io/overlays/mention/mention-shares-overlay/)
- [Mention Stats Overlay](https://apis.io/overlays/mention/mention-stats-overlay/)
- [Mention Tags Overlay](https://apis.io/overlays/mention/mention-tags-overlay/)
- [Mention Tasks Overlay](https://apis.io/overlays/mention/mention-tasks-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Mention Authentication](https://apis.io/security/mention/mention-authentication/)
- [Mention Domain Security](https://apis.io/security/mention/mention-domain-security/)
- [Mention Vulnerability Disclosure](https://apis.io/security/mention/mention-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Mention Scopes](https://apis.io/scopes/mention/mention-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Mention Plans Pricing](https://apis.io/plans/mention/mention-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Mention Rate Limits](https://apis.io/rate-limits/mention/mention-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Mention Finops](https://apis.io/finops/mention/mention-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://mention.com/)
- [Documentation](https://dev.mention.com/current/src/index.html)
- [Getting started](https://dev.mention.com/current/src/guidelines/Clients.html)
- [API reference](https://dev.mention.com/current/)
- [Pricing](https://mention.com/en/pricing/)
- [Sign up](https://social.agorapulse.com/mention-trial)
- [Support](https://en.support.mention.com/)
- [Blog](https://mention.com/en/blog/feed/)
- [Security](https://mention.com/en/terms-and-conditions/#security-policy)
- [Compliance](https://mention.com/en/terms-and-conditions/#security-policy)
- [Terms of service](https://mention.com/en/terms-and-conditions/#general-terms-and-conditions)
- [Privacy policy](https://mention.com/en/terms-and-conditions/#privacy-policy)
- [GitHub](https://github.com/mentionapp)
- [Status](https://status.mention.com/)

## API Evangelist rating

**Kin Score 60.4** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 48.8 |
| Contract governance | 18.2 |
| Operational transparency | 65.8 |
| Developer ergonomics | 58.9 |
| Access clarity | 73.7 |

**Agent readiness 32.2** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | documented (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 45.1** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/mention/](https://apis.io/providers/mention/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Mention: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/mention/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).

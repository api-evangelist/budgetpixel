# BudgetPixel

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

BudgetPixel is an AI creative platform offering credit-based access to 70+ generative media models for image, video, music and sound-effect generation, plus upscaling, lip sync, motion control, format conversion and content moderation. Alongside a free design-studio canvas, LoRA training and a public community feed, it ships a unified developer surface: a 74-operation OpenAPI 3.1 REST API at api.budgetpixel.com/v1 (private beta, bearer bpx_live_* keys, Premium/Pro/Ultra plans) with an asynchronous create-then-poll job model and a POST /v1/cost endpoint that quotes the exact credit charge before you commit; a first-party remote MCP server at mcp.budgetpixel.com/mcp exposing 12 tools over streamable HTTP with OAuth 2.1 + PKCE and Google SSO; an A2A agent card and a published Agent Skill; and llms.txt indexes. One credit balance, one set of conventions, and identical published prices across every surface.

**Areas:** Generative AI · AI Image Generation · AI Video Generation · AI Music Generation · Audio Generation · Content Moderation · Creative Tools · Design · Agent-Native · MCP · MCP Server · Artificial Intelligence

## BudgetPixel on APIs.io

Everything profiled for BudgetPixel is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/budgetpixel/](https://apis.io/providers/budgetpixel/)** — or go straight to one of the APIs or properties below.

### APIs (27)

- [BudgetPixel MCP Server](https://apis.io/apis/budgetpixel/budgetpixel-mcp-server/) — First-party hosted remote Model Context Protocol server (streamable HTTP, protocol 2025-06-18) giving agents 12 tools to generate images, video, music and…
- [BudgetPixel Docs Search MCP Server](https://apis.io/apis/budgetpixel/budgetpixel-docs-search-mcp-server/) — Read-only MCP server hosted on the BudgetPixel documentation site (Mintlify) giving agents two tools -- full-text documentation search and a sandboxed…
- [BudgetPixel Account API](https://apis.io/apis/budgetpixel/budgetpixel-account-api/) — Account and credit balance
- [BudgetPixel Alibaba API](https://apis.io/apis/budgetpixel/budgetpixel-alibaba-api/) — The Alibaba API from BudgetPixel — 12 operation(s) for alibaba.
- [BudgetPixel Audios API](https://apis.io/apis/budgetpixel/budgetpixel-audios-api/) — Music and sound-effect job status
- [BudgetPixel Black Forest Labs API](https://apis.io/apis/budgetpixel/budgetpixel-black-forest-labs-api/) — FLUX image models
- [BudgetPixel Bytedance API](https://apis.io/apis/budgetpixel/budgetpixel-bytedance-api/) — SeeDream image models
- [BudgetPixel ByteDance (SeeDance) API](https://apis.io/apis/budgetpixel/budgetpixel-bytedance-seedance-api/) — SeeDance video models
- [BudgetPixel Classification API](https://apis.io/apis/budgetpixel/budgetpixel-classification-api/) — Content classification — music genre detection
- [BudgetPixel Conversions API](https://apis.io/apis/budgetpixel/budgetpixel-conversions-api/) — Format conversion for images, video, and audio
- [BudgetPixel Google API](https://apis.io/apis/budgetpixel/budgetpixel-google-api/) — The Google API from BudgetPixel — 3 operation(s) for google.
- [BudgetPixel Images API](https://apis.io/apis/budgetpixel/budgetpixel-images-api/) — Image job status
- [BudgetPixel Kling API](https://apis.io/apis/budgetpixel/budgetpixel-kling-api/) — Kuaishou Kling video models
- [BudgetPixel Lip Sync API](https://apis.io/apis/budgetpixel/budgetpixel-lip-sync-api/) — The Lip Sync API from BudgetPixel — 3 operation(s) for lip sync.
- [BudgetPixel Models API](https://apis.io/apis/budgetpixel/budgetpixel-models-api/) — Discover available models and pricing
- [BudgetPixel Moderation API](https://apis.io/apis/budgetpixel/budgetpixel-moderation-api/) — Content moderation — NSFW rating and CSAM detection
- [BudgetPixel Motion Control API](https://apis.io/apis/budgetpixel/budgetpixel-motion-control-api/) — The Motion Control API from BudgetPixel — 3 operation(s) for motion control.
- [BudgetPixel Music API](https://apis.io/apis/budgetpixel/budgetpixel-music-api/) — Music generation models
- [BudgetPixel Open AI API](https://apis.io/apis/budgetpixel/budgetpixel-openai-api/) — The OpenAI API from BudgetPixel — 1 operation(s) for openai.
- [BudgetPixel Pruna AI API](https://apis.io/apis/budgetpixel/budgetpixel-pruna-ai-api/) — The Pruna AI API from BudgetPixel — 3 operation(s) for pruna ai.
- [BudgetPixel Social API](https://apis.io/apis/budgetpixel/budgetpixel-social-api/) — Publish posts to your BudgetPixel feed
- [BudgetPixel Sound Effects API](https://apis.io/apis/budgetpixel/budgetpixel-sound-effects-api/) — Text-to-sound-effect models (priced per second)
- [BudgetPixel Uploads API](https://apis.io/apis/budgetpixel/budgetpixel-uploads-api/) — Upload input media for image/video generation
- [BudgetPixel Upscaling API](https://apis.io/apis/budgetpixel/budgetpixel-upscaling-api/) — The Upscaling API from BudgetPixel — 5 operation(s) for upscaling.
- [BudgetPixel Videos API](https://apis.io/apis/budgetpixel/budgetpixel-videos-api/) — Video job status
- [BudgetPixel X AI API](https://apis.io/apis/budgetpixel/budgetpixel-xai-api/) — The xAI API from BudgetPixel — 2 operation(s) for xai.
- [BudgetPixel Mini Max API](https://apis.io/apis/budgetpixel/budgetpixel-mini-max-api/) — The Mini Max API from BudgetPixel — 1 operation(s) for mini max.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (2)

Model Context Protocol servers that expose the API to agents.

- [mcp](https://apis.io/servers/budgetpixel/mcp/)
- [mcp](https://apis.io/servers/budgetpixel/mcp-2/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Budgetpixel Openapi Overlay](https://apis.io/overlays/budgetpixel/budgetpixel-openapi-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Budgetpixel Authentication](https://apis.io/security/budgetpixel/budgetpixel-authentication/)
- [Budgetpixel Domain Security](https://apis.io/security/budgetpixel/budgetpixel-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Budgetpixel Scopes](https://apis.io/scopes/budgetpixel/budgetpixel-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Budgetpixel Plans Pricing](https://apis.io/plans/budgetpixel/budgetpixel-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Budgetpixel Rate Limits](https://apis.io/rate-limits/budgetpixel/budgetpixel-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.budgetpixel.com/)
- [Documentation](https://docs.budgetpixel.com/introduction)
- [Getting started](https://docs.budgetpixel.com/quickstart)
- [API reference](https://docs.budgetpixel.com/api-reference)
- [Pricing](https://budgetpixel.com/pricing)
- [Support](https://budgetpixel.com/contact)
- [Blog](https://budgetpixel.com/blog)
- [Terms of service](https://budgetpixel.com/terms)
- [Privacy policy](https://budgetpixel.com/privacy)
- [GitHub](https://github.com/budgetpixel-ai)

## API Evangelist rating

**Kin Score 54.0** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 51.8 |
| Contract governance | 18.2 |
| Operational transparency | 34.2 |
| Developer ergonomics | 64.3 |
| Access clarity | 63.2 |

**Agent readiness 54.3** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | yes |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | conformant |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 31.8** — AI & Biometrics

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/budgetpixel/](https://apis.io/providers/budgetpixel/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for BudgetPixel: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/budgetpixel/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).

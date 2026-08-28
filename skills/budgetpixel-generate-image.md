---
name: budgetpixel-generate-image
description: Generate or edit an image with BudgetPixel — pick a model from the live catalogue, price the request before committing credits, submit, and poll to a terminal state.
api: BudgetPixel REST API
base_url: https://api.budgetpixel.com/v1
operations:
  - listModels
  - estimateCost
  - getCredits
  - createImage_flux_2_pro
  - createImage_p_image_edit
  - getImage
  - uploadMedia
generated: '2026-08-28'
method: generated
source: openapi/budgetpixel-openapi.yaml + https://docs.budgetpixel.com/concepts/async-jobs
---

# Generate an image with BudgetPixel

Every operationId below was read from `openapi/budgetpixel-openapi.yaml`. The endpoint path
IS the model slug — there is no `model` body parameter on the REST API.

## Before you start

Requires an API key (`Authorization: Bearer bpx_live_...`) on a Premium, Pro or Ultra plan.
The developer API is in private beta.

## 1. Find a model and its price

`listModels` — `GET /v1/models`

Returns `name`, `type`, `credits_per_generation`, and where relevant `credits_per_unit`,
`unit_type`, `min_billable_seconds` and a `resolution_pricing` map. The `name` is the path
segment you POST to. Cache this for minutes to hours; it changes rarely and counts against
your rate limit.

## 2. Price the exact request — do not skip this

`estimateCost` — `POST /v1/cost`

Send the **exact body you intend to generate with**, plus `model`. It is computed by the
same code that bills, so it cannot disagree with the charge.

```json
{ "model": "flux-2-pro", "size": "2MP" }
```

Response: `{ "model": "...", "type": "image", "credits": 50, "exact": true }`

`exact: false` means `credits` is a **ceiling** (seedream sequential mode decides how many
images to return). Several models price on parameters, not just per image — `flux-2-pro` by
megapixels (0.5MP=15, 1MP=25, 2MP=50, 4MP=80), `grok-imagine-image-2` by `quality` × `size`,
`seedream-5.0-pro` by output size plus 5 credits per reference image after the first.

Check headroom with `getCredits` — `GET /v1/account/credits` → `total_available`. A request
that would exceed it is rejected before the job is created (402), not overdrafted.

## 3. Supply input media, if editing

`uploadMedia` — `POST /v1/uploads`. Returns `{ url, content_type, bytes, expires_in }`; the
URL lives roughly 24 hours. Free and unmetered, but capped at **60 uploads/minute per
account** on top of the normal limits. Pass the returned URL into the operation's image or
reference-image field. `createImage_p_image_edit` (`POST /v1/images/p-image-edit`) is the
dedicated edit route.

## 4. Submit

`createImage_flux_2_pro` — `POST /v1/images/flux-2-pro`

```json
{ "prompt": "...", "num_images": 1 }
```

Returns `{ "id": "img_a1b2c3d4e5f6", "status": "pending" }` immediately. For a multi-image
request the platform reserves `max_images × credits_per_generation` up front and charges
only for images actually returned.

**Submit exactly once.** There is no idempotency key and no cancel, refund or delete
operation anywhere in this API. A retried POST after an ambiguous timeout creates a second
charged job, and REST has no history endpoint to detect the duplicate.

## 5. Poll to a terminal state

`getImage` — `GET /v1/images/{job_id}`

Poll every few seconds — images usually finish in seconds. Statuses run
`pending → starting → processing → completing → succeeded`, with `failed` and `timeout` as
the other terminal states. Stop polling on any terminal state. On `succeeded`, read the
`images[]` array of `{ position, url }`.

Polling does not count against your per-plan concurrency cap, but it does count against the
600/min per-key request limit.

## Errors you must branch on

| Status | Shape | What to do |
|---|---|---|
| 400 with `restriction_reason` | `{"error": "<string>", "restriction_reason": "<enum>"}` | Content moderation blocked the **prompt or input**. Reword and retry. Not an account problem. |
| 400 without `restriction_reason` | `{"error":{"type","code","message"}}` | Client bug — `model_not_available`, malformed body. Do not retry unchanged. |
| 401 | `missing_api_key` / `invalid_api_key` | Fix the header or mint a new key. |
| 402 | billing | Insufficient credits. Hard stop — top up. |
| 403 | `api_access_not_enabled` / `account_banned` | Plan or account gate. Hard stop, do not retry. |
| 429 | `rate_limited` | Sleep the **full** `Retry-After`. Rejected requests still count against the window. |

Note the envelope trap: on a moderation 400, `error` is a **string**, not an object, so
`response.error.code` reads undefined.

`restriction_reason` values: `input_explicit_adult`, `input_upload_nudity`,
`input_celebrity_likeness`, `strict_model_nsfw`, `input_csam`.

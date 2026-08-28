---
name: Budgetpixel
description: Use when building applications that generate images, video, music, or sound effects via API; when integrating BudgetPixel into workflows; when helping users understand API authentication, async job polling, credit pricing, or model selection; when troubleshooting generation failures or rate limits.
metadata:
    mintlify-proj: budgetpixel
    version: "1.0"
---

# BudgetPixel Skill

## Product summary

BudgetPixel is a unified API for generating images, video, music, and sound effects using models from Black Forest Labs (FLUX), ByteDance (SeeDream, SeeDance), Alibaba (Qwen, Wan), Google (Nano Banana, Lyria), and others. Agents use it to build applications that create media programmatically, poll async jobs for results, and manage credit-based billing.

**Key files & endpoints:**
- Base URL: `https://api.budgetpixel.com/v1`
- Authentication: Bearer token in `Authorization` header (`bpx_live_xxx`)
- Job creation: `POST /v1/images/{model}`, `POST /v1/videos/{model}`, `POST /v1/audios/{model}`
- Job polling: `GET /v1/images/{id}`, `GET /v1/videos/{id}`, `GET /v1/audios/{id}`
- Model catalog: `GET /v1/models?type=image|video|audio`
- Cost estimation: `POST /v1/cost` (send the same body as generation with `model` added)
- Credit balance: `GET /v1/account/credits`
- Media upload: `POST /v1/uploads` (multipart/form-data, max 50 MB)

**Primary docs:** https://docs.budgetpixel.com

## When to use

Reach for this skill when:
- Building an application that generates images, video, music, or sound effects on demand or at scale
- Integrating BudgetPixel into a backend service or workflow
- Helping users understand API authentication, rate limits, or credit pricing
- Debugging generation failures, content moderation blocks, or job timeouts
- Selecting the right model for a task (e.g., text-to-image vs. image-to-image, video resolution pricing)
- Estimating costs before running expensive generations
- Handling async job polling and result retrieval
- Uploading reference media for image-to-image or video-to-video workflows

Do not use this skill for: account management, subscription changes, dashboard-only operations, or MCP server setup (use the MCP docs for agent-to-account connections).

## Quick reference

### Authentication & setup
- Create an API key from the BudgetPixel dashboard (Premium/Pro/Ultra plans only; private beta)
- Store the key in an environment variable: `BUDGETPIXEL_API_KEY`
- Never embed keys in client-side code or public repos
- All requests require: `Authorization: Bearer $BUDGETPIXEL_API_KEY`

### Generation workflow
1. **List models** — `GET /v1/models?type=image` (or `video`, `audio`)
2. **Estimate cost** — `POST /v1/cost` with the exact request body + `model` field
3. **Create job** — `POST /v1/images/{model}` (or `/videos/{model}`, `/audios/{model}`)
4. **Poll status** — `GET /v1/images/{job_id}` every few seconds until `status` is `succeeded`, `failed`, or `timeout`
5. **Retrieve results** — On success, `images` array (images), `video_url` (video), or `audio_url` (music/SFX)

### Media input formats
All media parameters accept one of:
- **Uploaded-file URL** — `POST /v1/uploads` first, then pass the returned URL
- **Public URL** — Direct `https://` link (must be publicly reachable)
- **Data URI** — `data:image/png;base64,iVBORw0...`
- **Raw base64** — Bare base64 string (no prefix)

### Pricing essentials
- **Charged only on success** — failed, timed-out, or blocked jobs cost nothing
- **Images** — per output image (`credits_per_generation`); multi-image requests charged per image
- **Video** — per second by resolution (e.g., SeeDance 2.0: 480p = 100, 720p = 220, 1080p = 550 credits/sec)
- **Music** — flat per track (Music 3.0 = 200, Lyria 3 = 100) or per second for Sonilo Music (4 credits/sec, 10-sec minimum)
- **Sound effects** — per second (5 credits/sec from text, 15 credits/sec from video; 3-sec minimum)
- **Utility** — conversions (2–10 credits), posts (10 credits)
- **Reserve & charge** — Multi-image requests reserve max cost upfront, charge only for images returned

### Rate limits
- **Per API key:** 600 requests/minute
- **Per source IP:** 1200 requests/minute
- **Uploads:** 60 uploads/minute per account (rolling window, on top of above)
- **Response headers:** `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`
- **On 429:** Respect `Retry-After` header; rejected requests still count against the window

### Job lifecycle
```
pending → starting → processing → completing → succeeded
                                              ↘ failed
                                              ↘ timeout
```
Terminal states: `succeeded`, `failed`, `timeout` — stop polling once reached.

## Decision guidance

| Scenario | Choose | Why |
|----------|--------|-----|
| **Multiple images from one prompt** | SeeDream 4.5/5.0-lite with `sequential_image_generation: "auto"` | Model decides count; billed per image returned |
| **Exact number of image variations** | SeeDream 5.0-Pro with `num_images: N` | Each is independent; full control |
| **Fast, cheap images** | FLUX 2 Klein or Qwen Image | Lower credit cost; good for high volume |
| **High-quality, detailed images** | FLUX 2 Pro or SeeDream 5.0-Pro | Premium pricing; best aesthetics |
| **Text-to-video** | SeeDance 2.0 or Wan 3.0 | Multimodal; support reference media |
| **Video editing (restyle/transform)** | SeeDance 2.0 or Wan 2.7 | Accept `video` input; apply prompt to clip |
| **Video with reference media** | SeeDance 2.5 or Wan 3.0 | Support up to 15 reference images, 5 reference videos |
| **Cheap video** | SeeDance 2.0-Mini or P-Video | Lower per-second rates; 480p/720p only |
| **Music with lyrics** | Music 2.6 or Mureka V9 | Accept `lyrics` parameter; full songs |
| **Exact-length instrumental** | Sonilo Music | Priced per second; 5–360 seconds |
| **Sound effects from text** | Sonilo SFX | 5 credits/sec; 3-sec minimum |
| **Sound effects synced to video** | Sonilo Video SFX | 15 credits/sec of input; returns video + SFX mixed |
| **Cost estimation before generation** | `POST /v1/cost` | Same logic as billing; never disagrees |
| **Check available credits** | `GET /v1/account/credits` | Returns monthly/extra breakdown |

## Workflow

### Generate an image
1. **Authenticate** — Set `Authorization: Bearer $BUDGETPIXEL_API_KEY`
2. **List models** — `GET /v1/models?type=image` to see available models and prices
3. **Pick a model** — Choose based on quality, speed, and cost (e.g., `flux-2-pro`, `seedream-5.0-pro`)
4. **Estimate cost** — `POST /v1/cost` with `{"model": "flux-2-pro", "prompt": "...", "num_images": 1, "size": "1MP"}`
5. **Check balance** — `GET /v1/account/credits` to ensure sufficient credits
6. **Create job** — `POST /v1/images/flux-2-pro` with prompt, size, aspect ratio, etc.
7. **Poll status** — `GET /v1/images/{job_id}` every 2–5 seconds until `status` is `succeeded`
8. **Retrieve URLs** — Extract `images[].url` from the succeeded response (valid ~1 hour; re-poll for fresh URLs)

### Generate video with reference media
1. **Upload reference** — `POST /v1/uploads` with a local image/video file (returns ephemeral URL)
2. **List video models** — `GET /v1/models?type=video`
3. **Estimate cost** — `POST /v1/cost` with model, prompt, resolution, length, and reference media URLs
4. **Create job** — `POST /v1/videos/seedance-2.0` with prompt, `reference_images: [url]`, resolution, length
5. **Poll** — `GET /v1/videos/{job_id}` every 10–30 seconds (video takes longer)
6. **Retrieve** — Extract `video_url` on success

### Handle a content moderation block
1. **Receive 400 error** with `restriction_reason` field (e.g., `input_explicit_adult`, `input_celebrity_likeness`)
2. **Branch on `restriction_reason`** — don't retry the same prompt/image
3. **Reword prompt** or change input media to avoid the restriction
4. **Retry** — the block is about content, not your account; a modified request may succeed
5. **Log the block** — track which restrictions are common in your workflow

### Manage rate limits
1. **Monitor response headers** — Check `X-RateLimit-Remaining` after each request
2. **Poll gently** — Every 2–5 seconds for images, 10–30 seconds for video (not in a tight loop)
3. **Cache stable data** — `GET /v1/models` changes rarely; cache for hours
4. **On 429** — Read `Retry-After` header, sleep that many seconds, then resume
5. **Add jitter** — If multiple workers share a key/IP, stagger retries to avoid thundering herd

## Common gotchas

- **Embedding API keys in code** — Keys are secrets; store in environment variables or a secrets manager. Anyone with your key can spend your credits.
- **Polling in a tight loop** — This burns through rate limits and keeps you rate-limited indefinitely. Poll every few seconds, not milliseconds.
- **Ignoring `Retry-After` on 429** — Rejected requests still count against the window. Respect the header or you stay rate-limited.
- **Assuming job results are permanent** — Image/video URLs are short-lived (~1 hour); re-poll the status endpoint for fresh URLs. Underlying objects expire 24 hours after generation.
- **Uploading the same file repeatedly** — Uploads are ephemeral (24 hours). If you need a file again, re-upload it or use a public URL.
- **Not checking balance before expensive requests** — Requests that exceed available credits are rejected before the job is created. Call `GET /v1/account/credits` first.
- **Forgetting to estimate cost for parameter-dependent models** — FLUX 2 Pro, SeeDream 5.0-Pro, and video models with resolution pricing have complex pricing. Always use `POST /v1/cost` to avoid surprises.
- **Mixing incompatible parameters** — Some models don't support certain combinations (e.g., SeeDance 2.0 can't use `image` + `reference_images` together). Check the model's API reference page.
- **Passing private/internal URLs** — Media parameters only accept publicly reachable `https://` URLs. Loopback and internal addresses are rejected.
- **Resizing input images yourself** — Images over 2048 px or 4 MB are downscaled automatically. Send full-resolution images; there's no benefit to shrinking them first.
- **Assuming all models are API-available** — The model catalog is the source of truth. Calling an endpoint for a model not in `GET /v1/models` returns `400 model_not_available`.
- **Not handling terminal job states** — `succeeded`, `failed`, and `timeout` are terminal. Stop polling once you see one; polling a terminal job repeatedly wastes rate-limit quota.
- **Forgetting that failed jobs cost nothing** — You're only charged on success. Use this to your advantage: estimate cost first, then generate, and retry on failure without penalty.

## Verification checklist

Before submitting work with BudgetPixel:

- [ ] **API key is set** — Stored in an environment variable, not hardcoded
- [ ] **Authentication header is correct** — `Authorization: Bearer $BUDGETPIXEL_API_KEY`
- [ ] **Model exists** — Confirmed in `GET /v1/models` response
- [ ] **Cost estimated** — Ran `POST /v1/cost` with the exact request body
- [ ] **Balance checked** — `GET /v1/account/credits` shows sufficient credits
- [ ] **Media inputs are valid** — Public URLs, uploaded-file URLs, or base64 (not private/loopback addresses)
- [ ] **Parameters match the model** — Checked the model's API reference page for required/optional fields
- [ ] **No content moderation blocks** — Prompt and input media don't violate content policy
- [ ] **Polling interval is reasonable** — Every 2–5 seconds for images, 10–30 seconds for video
- [ ] **Terminal state handling** — Code stops polling on `succeeded`, `failed`, or `timeout`
- [ ] **Rate limits respected** — Not polling in a tight loop; respecting `Retry-After` on 429
- [ ] **Results are re-fetched** — Code doesn't assume image/video URLs are permanent; re-polls for fresh URLs if needed
- [ ] **Error handling is complete** — Code branches on `error.code` and `restriction_reason` for content blocks

## Resources

**Comprehensive page listing:** https://docs.budgetpixel.com/llms.txt

**Critical documentation pages:**
- [Quickstart](https://docs.budgetpixel.com/quickstart) — End-to-end example of creating and polling an image job
- [Async jobs](https://docs.budgetpixel.com/concepts/async-jobs) — Job lifecycle, polling patterns, error handling
- [Pricing & credits](https://docs.budgetpixel.com/concepts/pricing-and-credits) — Per-model pricing, cost estimation, billing rules
- [Models](https://docs.budgetpixel.com/concepts/models) — Model catalog, video generation modes, multi-image requests
- [Media inputs](https://docs.budgetpixel.com/concepts/media-inputs) — Upload, URL, base64, and data URI formats
- [Rate limits](https://docs.budgetpixel.com/concepts/rate-limits) — Request limits, response headers, retry strategy
- [Authentication](https://docs.budgetpixel.com/authentication) — API key management and security

---

> For additional documentation and navigation, see: https://docs.budgetpixel.com/llms.txt
---
name: budgetpixel-generate-video
description: Generate or edit a video with BudgetPixel — price it first (video is the expensive surface and reference-video input is often billed too), submit, then poll for minutes.
api: BudgetPixel REST API
base_url: https://api.budgetpixel.com/v1
operations:
  - listModels
  - estimateCost
  - getCredits
  - uploadMedia
  - createVideo_seedance_2_5
  - createVideo_wan_3_0_video
  - createVideo_kling_v3_0_pro
  - getVideo
  - createVideoUpscale_topazlabs_video_upscale
  - getVideoUpscaleJob
generated: '2026-08-28'
method: generated
source: openapi/budgetpixel-openapi.yaml + https://docs.budgetpixel.com/concepts/pricing-and-credits
---

# Generate a video with BudgetPixel

Video is where an agent can spend real money by accident. Price every request.

## Why pricing matters more here

Video is billed **per second of output, by resolution** — and several models **also bill the
reference video you pass in**:

- `seedance-2.5`: 480p=150, 720p=330, 1080p=750 credits/sec output, **plus reference-video
  input at half the output rate**. A 5s 720p generation guided by an 8.2s clip is
  `5×330 + 9×160 = 3,090` credits.
- `seedance-2.0` / `-fast` / `-mini`: video-edit input billed at **half** the output rate.
- `wan-2.7-video`: 720p=100, 1080p=150 credits/sec, and input video billed at the **full**
  rate with no discount — and video edit takes its output length **from the input clip**,
  ignoring `length_seconds`. An 8.2s edit at 720p costs `9×100 + 9×100 = 1,800`.
- `wan-3.0-video`: output seconds only, 480p=60 / 720p=120 / 1080p=240 (prime tier
  85/170/340). **All input media is free.** The cheapest choice for reference-driven work.
- `minimax-h3`: 160 credits/sec output **and** 160 per second of reference-video input.

Always call `estimateCost` (`POST /v1/cost`) with the exact body plus `model` before you
submit, and check `getCredits` (`GET /v1/account/credits`) for headroom.

## Flow

1. `listModels` — `GET /v1/models`. Read `credits_per_unit`, `unit_type: second` and
   `resolution_pricing`.
2. `uploadMedia` — `POST /v1/uploads` for any start frame, end frame, reference image or
   input clip. Returns a URL valid ~24h. Capped at 60 uploads/min per account.
3. `estimateCost` — `POST /v1/cost`.
4. Submit to the model's own endpoint. The path is the model slug:
   - `createVideo_seedance_2_5` — `POST /v1/videos/seedance-2.5`
   - `createVideo_wan_3_0_video` — `POST /v1/videos/wan-3.0-video`
   - `createVideo_kling_v3_0_pro` — `POST /v1/videos/kling-v3.0-pro`

   ```json
   { "prompt": "...", "length_seconds": 5, "resolution": "720p" }
   ```

   Returns `{ "id": "...", "status": "pending" }`.
5. `getVideo` — `GET /v1/videos/{job_id}`. **Poll every 10–30 seconds**, not in a tight
   loop; video takes minutes. `VideoJob` carries `started_at`, `updated_at` and
   `completed_at` alongside `status` and, on success, `video_url`.

Optionally upscale: `createVideoUpscale_topazlabs_video_upscale` —
`POST /v1/video-upscales/topazlabs-video-upscale`, priced per second of **input** video
(1080p = 15 @30fps / 30 @60fps, 4K = 60 / 120), input up to 20s. Poll with
`getVideoUpscaleJob` — `GET /v1/video-upscales/{id}`.

## Safety rules

- **Nothing here is reversible.** No cancel, no refund, no delete — the API has no DELETE
  verb at all. Once a job succeeds the credits are spent.
- **No idempotency key.** Submit once. On an ambiguous failure, poll for the job rather than
  re-POSTing; a duplicate submission is a second full charge.
- Concurrency is capped per plan (Premium 5, Pro 7, Ultra 10) and **never appears in the
  `X-RateLimit-*` headers** — you cannot detect it from response headers, only from behaviour.
- Credits are charged on success only. `failed` and `timeout` cost nothing.

## Errors

Same envelope and branches as `budgetpixel-generate-image` — see
`errors/budgetpixel-problem-types.yml`. The one to watch on video is 402 (insufficient
credits), which is declared on 7 operations and fires *before* the job is created.

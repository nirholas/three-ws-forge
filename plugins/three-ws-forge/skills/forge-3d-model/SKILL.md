---
name: forge-3d-model
description: Generate a textured 3D GLB from text, images or a sketch, and auto-rig an existing GLB, with the @three-ws/forge SDK. Use when code or an agent needs a 3D asset or a rigged character generated through three.ws Forge.
---

# Generate and rig 3D models with three.ws Forge

`@three-ws/forge` is a dependency-free JavaScript SDK (Node 18+ and browsers) over the three.ws Forge API. The `draft` tier runs on the free lane; `standard` and `high` tiers are billed to account credits, or pay-per-call in USDC over x402.

```bash
npm install @three-ws/forge
```

## API

- `forge(promptOrInput, opts?)` submits a job and polls until it is done. Input is a prompt string or `{ prompt?, images?, aspectRatio? }`. Passing `images` switches to image to 3D. Returns `{ jobId, creationId, status, glbUrl, viewerUrl, path, tier, backend, etaSeconds, estimatedCredits, durable }`.
- `rig(glbUrl, opts?)` auto-rigs a GLB URL and returns the same result shape.
- `catalog()` returns the live tier, backend and path matrix (`GET /api/forge?catalog=1`). Read it instead of hardcoding backend ids.
- `getJob(jobId, { providerKey? })` polls one job.
- `createForge({ baseUrl?, fetch?, apiKey?, providerKey?, headers? })` builds a client with the same four methods. Pass a payment-aware `fetch` here to settle x402 challenges.

Options: `tier` (`'draft' | 'standard' | 'high'`), `path` (`'image' | 'geometry' | 'sketch'`), `backend`, `payWith` (`'credits'` default, or `'x402'`), `providerKey` (bring-your-own Meshy or Tripo key for the geometry path), `onProgress(job)`, `pollIntervalMs`, `timeoutMs`, `signal`.

With `payWith: 'x402'` the call goes to `/api/x402/forge`, which picks its own lane and rejects `path`, `backend` and `providerKey`.

Errors are `ThreeWsError` with a `code`. Common ones: `invalid_input` (rejected before any request), `generation_unavailable` (503, retry or change tier), `unauthorized` (401, credits lane needs a signed-in account), `insufficient_credits` (402), `payment_required` (402, `accepts` holds the challenge, raised as `PaymentRequiredError`). The origin can be set with `baseUrl` or env `THREE_WS_BASE_URL`.

## Worked example

```js
import { forge, rig } from '@three-ws/forge';

const model = await forge('a chrome robot with neon trim', {
  tier: 'draft',
  onProgress: (j) => console.log(j.status),
});
console.log(model.glbUrl, model.viewerUrl);

const rigged = await rig(model.glbUrl);
console.log(rigged.glbUrl);
```

## Spending

The free `draft` tier costs nothing. Before any call with `payWith: 'x402'` or a paid tier that settles USDC, tell the user the tier and quoted amount and wait for an explicit yes; settling a 402 moves real funds.

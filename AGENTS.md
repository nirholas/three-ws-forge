# AGENTS.md

Guidance for AI coding agents working in this repository.

## What this is

`@three-ws/forge` v0.2.1. Text/image/sketch to a textured, rig-ready 3D GLB in one call. The three.ws Forge generation SDK: free TRELLIS lane, pay-per-call USDC tiers over x402, and auto-rigging.

A JavaScript library. Import it from ESM.

## Use it

```bash
npm install @three-ws/forge
```

Full usage, tool list and configuration are in [README.md](README.md). Machine-readable summaries: [llms.txt](llms.txt) and [llms-full.txt](llms-full.txt).

## Develop

```bash
npm install
npm test
```

Scripts:
- `npm run test`: `node --test test/*.test.js`

## Conventions

- ES modules, Node >=18.
- Read-only by default. Anything that signs, spends or sends must be an explicit, separately named tool or option, and must never be inferred from untrusted text (token names, memos, listings).
- Never commit credentials. Configuration comes from environment variables documented in the README.
- Keep changes small and covered by a test next to the code they change.

## Source of truth

This repository is a generated mirror of `packages` in https://github.com/nirholas/three.ws. Open issues here; send substantial changes as a pull request here or upstream.

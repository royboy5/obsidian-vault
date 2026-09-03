---
tags:
role: infrastructure
---
# Shared Packages

> Code shared across 2+ apps lives in `packages/`. Never created speculatively — added only when duplication actually shows up (see [[Monorepo & Toolchain]] rule of thumb).

| Package | Purpose | Used by |
|---|---|---|
| `packages/auth` | Better Auth client, JWT parsing/validation, org-context middleware, role guards (`requireOrgAdmin`, `requireCoach`…) | every `*-api` |
| `packages/billing` | Stripe entitlement checks, seat assignment | every `*-api` |
| `packages/identity` | HTTP client for identity-api (profiles, orgs, memberships) — product APIs never query the identity DB directly | every `*-api` |
| `packages/ai` | System prompts + provider swap (Ollama dev ↔ Claude prod) | `ai-api` |
| `packages/types` | OpenAPI-generated TypeScript types | every `*-web`, every `*-api` |
| `packages/dart-types` | OpenAPI-generated Dart types | every `mobile-*` |
| `packages/ui` | Shared React components — app switcher, nav, auth screens | every `*-web` |
| `packages/canvas` | Shared Flutter drawing tool (editor + read-only viewer + keyframe engine) | `mobile-coach` (full editor), others (viewer only, future) |
| `packages/openapi` | `volleyball-suite.yaml` — single source of truth for the API contract | generates `types` + `dart-types` |

## Related
[[Monorepo & Toolchain]] · [[Auth Middleware & Security]]

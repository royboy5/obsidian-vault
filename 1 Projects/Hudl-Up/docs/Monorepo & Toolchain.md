---
tags:
role: infrastructure
---
# Monorepo & Toolchain

## Why Moon

Lightweight, language-agnostic task runner — works cleanly with both Node (Hono APIs, React web) and Dart/Flutter (mobile) without plugins. Fast incremental builds via an affected-task graph. Each app/package just needs its own `moon.yml`.

## Moonrepo Setup
### Installation and Workspace

- [[Moonrepo Installation and Global Setup]]
- [[Moonrepo Workspace Initialization]]
### Toolchain Setup

- [[Moonrepo Toolchain - PNPM]]
- [[Moonrepo Toolchain - Node TypeScript]]
- [[Moonrepo Toolchain - TypeScript Config]]
- [[Moonrepo Toolchain - Biome]]
- [[Moonrepo Toolchain - React Vite]]
- [[Moonrepo Toolchain - Dart Flutter]]

## Workspace layout
```
hudl-up/
├── apps/
│   ├── identity-api/          Hono — foundation, everything depends on it
│   ├── club-manager-web/      React + Vite
│   ├── club-manager-api/      Hono
│   ├── mobile-club-manager/   Flutter
│   ├── coach-web/             React + Vite
│   ├── coach-api/             Hono
│   ├── mobile-coach/          Flutter
│   ├── ai-web/                React + Vite
│   ├── ai-api/                Hono
│   ├── mobile-ai/             Flutter
│   ├── mobile-player-portal/  Flutter — free, no web app, no own DB
│   └── admin-web/             React + Vite — internal super admin only
├── packages/                  → see [[Shared Packages]]
├── nginx/nginx.conf
└── docker-compose.yml
```

## Toolchain per app type
| App type             | Language   | Task runner config                                                            | Key libs                                                                             |
| -------------------- | ---------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `*-api` (Hono)       | TypeScript | `moon.yml` → `node --watch src/main.ts` (dev), `tsc` (build)                  | Drizzle, Better Auth (identity only), Vercel AI SDK (ai-api only)                    |
| `*-web` (React)      | TypeScript | Vite dev/build                                                                | react-big-calendar, html5-qrcode, react-signature-canvas, product-specific libs      |
| `mobile-*` (Flutter) | Dart       | `moon.yml` → `flutter run` (dev), `flutter build apk` (build), `flutter test` | `packages/canvas`, `mobile_scanner`, `qr_flutter`, `signature_pad`, MediaPipe plugin |

## Common commands
```bash
moon run :build                   # build everything affected
moon run :test                    # test everything affected
moon run club-manager-api:dev     # run one app
```

## Type sharing (no manual sync between TS and Dart)
```
packages/openapi/volleyball-suite.yaml   ← single source of truth, edit only this
        ↓ moon run openapi:generate
packages/types/src/                      ← generated TypeScript
packages/dart-types/lib/                 ← generated Dart
```

## Infra alongside the monorepo
- **Docker Compose** — one Postgres container per product (`postgres-identity`, `postgres-club-manager`, `postgres-coach`, `postgres-ai` — the last with pgvector) + `identity-api`
- **Nginx** — one server block per subdomain (`identity.`, `club.`, `coach.`, `ai.`, `admin.`), routes `/api/` to the Hono port and `/` to the Vite dev port
- **CI/CD** — planned around `moon`'s affected-graph so only changed apps rebuild/test/deploy

## Rule of thumb for adding a package
Only promote code to `packages/` once **2+ apps** actually need it — see [[Shared Packages]].

## Related
[[Shared Packages]] · full Docker Compose + Nginx config → ?

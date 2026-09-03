---
tags: [project/volleyball-suite, domain/ai, type/hub-note]
role: product
---

# AI Assistant
↩ [[Volleyball Suite]] (canvas)

> Rules Q&A (RAG over rulebooks), approved-technique coaching, and on-device video/pose analysis. Third product built (Step 6) — deliberately last, since it's most useful once real org/team data exists to ground it.

## At a glance
- **Apps:** `ai-web` (React) · `ai-api` (Hono) · `mobile-ai` (Flutter)
- **DB:** `postgres-ai` (Postgres + **pgvector**)
- **AI SDK:** Vercel AI SDK — one file (`packages/ai`) swaps provider via `NODE_ENV`

| Env | Chat / Vision | Embeddings |
|---|---|---|
| Dev | Ollama (`llama3.2`, `llava`) | `nomic-embed-text` |
| Prod | Claude (`claude-sonnet-4-5`) | `voyage-3` |

## Core mechanics
- **RAG:** org uploads rulebook PDFs → chunked, embedded, tagged by org (FIVB/NCAA/USAV/NFHS) + season → AI always cites org + section
- **Technique guardrail:** AI only teaches from an admin-curated, approved technique library — explicitly says so if something isn't approved
- **Video pipeline:** MediaPipe runs **on-device** (Flutter) — only structured joint-angle JSON is sent to the API; raw video never leaves the device
- **MCP:** wraps the rules DB / technique library as tool calls (`search_rules`, `get_technique`, `list_rule_changes`)

## Key data models
Rulebook chunks + embeddings (pgvector), approved technique library, frame-analysis JSON — all detailed in [[ai]]

## Related
[[Coach]] (AI reviews animated plays) · [[Monorepo & Toolchain]] (provider swap pattern)

---
status: proposed
created: 2026-08-31 23:30
updated: 2026-08-31 23:30
tags:
  - adr
---

## ADR: Biomejs

This is worth answering properly, because it's not really "ESLint is bad" — it's a specific mismatch between how flat config works and how task runners like moon expect a monorepo to behave. A few concrete things, some of which we personally hit:

**1. moonrepo's own official docs are stuck in the pre-flat-config era.** I checked their published ESLint guide, and it still tells people to create a tsconfig.eslint.json, extend shared compiler options, add parserOptions to the root-level config — that's the exact eslintrc-era pattern that broke for you. So the first friction point isn't architectural at all, it's that moon's reference example hasn't been updated for ESLint 9/10, and anyone following it walks straight into what you hit.

**2. Flat config only resolves one config per invocation, and that model fights monorepo task runners.** Under flat config, the CLI only ever loads one config depending on where you invoke it — run it at the monorepo root and only the root config loads; run it in a nested directory and a different config loads, with paths inherited from the root resolving relative to the wrong directory entirely. moon's whole model is "run this exact task, scoped to this exact project, with declared inputs for caching" — which assumes clean per-project boundaries. ESLint's config resolution doesn't cleanly respect those boundaries; it walks the directory tree and there's ambiguity about which config actually governs a given file depending on cwd.

**3. It genuinely doesn't scale cleanly across workspace packages yet.** There's an open ESLint issue (from a few months ago) pointing out that in a monorepo, each workspace package has its own tsconfig scoping type-checking, but ESLint doesn't respect that boundary when run across the whole repo, so memory grows unboundedly — the proposed fix is for ESLint to natively discover workspace packages and lint each one in isolation, which doesn't exist yet.

**4. Shared configs need manual cache-wiring, because ESLint has no concept of "this affects downstream caches."** Turborepo's own docs show the same workaround moon needs: using dependsOn with ^lint ensures that changes to a shared package like @repo/eslint-config invalidate the cache for lint tasks, even though the config package doesn't have a lint script itself. That's not automatic — you have to know to wire it. Combine that with a shared config package that itself goes stale (which is literally what happened to you with `eslint-config-moon`), and you get a single point of fragility sitting in the middle of every project's cache graph.

**5. Config-file module resolution is coupled to the nearest `package.json`, which is exactly what caused your `module.exports` crash.** That's not monorepo-specific, but a repo with multiple `package.json` files at different levels (which every moon workspace has) makes it more likely you'll have a mismatch between where a config file lives and which `package.json`'s `"type"` field governs it.

**Why Biome specifically:** two separate things are going on. First, the actual recommendation source — it's the moonrepo GitHub org itself, via the now-deprecated `eslint-config-moon`/`prettier-config-moon` packages, telling people to move to Biome instead. Slightly ironic given point #1 — their own `moon` task-runner example docs haven't caught up with their own tooling team's advice. Second, on the merits: Biome sidesteps this whole category of problem structurally — one Rust binary, one config file, lint+format+import-sort built in, no satellite shareable-config npm package that can independently bit-rot, and no directory-walking config resolution coupled to `package.json`'s module type. It's not that Biome's rules are inherently better than ESLint's (we already established the plugin ecosystem is narrower) — it's that the *infrastructure* around it doesn't have the failure modes that make ESLint specifically painful to wire into a cached, per-project task graph like moon's.

## Context
- Describe the context and problem statement that this decision addresses.

## Decision
- Describe the decision that was made

## Consequences
- Describe the positive and negative consequences of this decision


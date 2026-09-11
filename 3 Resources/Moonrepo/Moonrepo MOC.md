---
created: 2026-06-23 23:43
updated: 2026-09-10 23:14
tags:
  - resource
  - moc
  - monorepo
---
## Moonrepo MOC

*This is my central map and table of contents for all knowledge related to [Moonrepo](https://moonrepo.dev/docs).  Its purpose is to structure my atomic notes and guide my learning.*

> **What is moonrepo?**
> 
> Moonrepo is a monorepo tool used to assist with management, organization, orchestration, and notification of code.  The tool is written in [Rust](https://rust-lang.org/).  Supports multiple programming languages and dependency managers, so a repo that is composed of different languages and tools can still work in unison.  Uses [proto](https://moonrepo.dev/proto) as a version management for your programming languages and tools.

Broken vault notes were dropped. Those topics now point at [moon v2 docs](https://moonrepo.dev/docs/concepts) until local notes exist.

 ---
## Getting Started & Setup
These blueprints are strictly tool-agnostic. You execute these *once* to provision a system and initialize a blank repository container.

- **System Provisioning:** [[Moonrepo Installation and Global Setup]] — Local machine CLI environment setup using `proto`.
- **Agnostic Foundation:** [[Moonrepo Workspace Initialization]] — Laying down the base container and global workspace boundaries.

### ⚡ Automation Scripts
* [[New Moonrepo Workspace Script]] — Dynamic Templater runbook to instantly output execution-ready shell setups.

---

## JS default path (after init)

Use these together for a pnpm + TypeScript workspace (Vite SPA and/or Node API). Do not also install ESLint/Prettier unless the repo is not on Biome.

* [[Moonrepo Toolchain - PNPM]] — Workspace packages and proto pins.
* [[Moonrepo Toolchain - TypeScript Config]] — Shared tsconfig + inherited `typecheck` (`tsc --noEmit`).
* [[Moonrepo Toolchain - Node TypeScript]] — Hono/tsdown-style API apps (`stack: backend`).
* [[Moonrepo Toolchain - React Vite]] — React Vite + Vitest SPAs (`stack: frontend`).
* [[Moonrepo Toolchain - Biome]] — Lint/format for TypeScript projects.
* [[Moonrepo Task Dependencies]] — `dependsOn` vs task `deps`; start an API with a SPA.

---

## 📦 Core Pillars & Concepts

### 1. Workspace & Toolchain
The global root environment that manages cross-project configurations, shared languages, and environment-wide constraints.

* [Workspace](https://moonrepo.dev/docs/concepts/workspace) / [workspace.yml](https://moonrepo.dev/docs/config/workspace) — projects glob, VCS, pipeline. Local setup: [[Moonrepo Workspace Initialization]].
* [Toolchain](https://moonrepo.dev/docs/concepts/toolchain) / [toolchains.yml](https://moonrepo.dev/docs/config/toolchain) — how moon installs and versions languages. Pins live in `.prototools` ([proto](https://moonrepo.dev/docs/proto)).

#### 🔌 Language & Package Integration Recipes
These modular notes track how specific language runtimes and package ecosystems plug into moonrepo:

* **JavaScript/Node:** [[Moonrepo Toolchain - PNPM]] — Layering node package topologies across the workspace.
* **JavaScript/Node:** [[Moonrepo Toolchain - Node TypeScript]] — Node apps, tsx, tsdown.
* **JavaScript/Node:** [[Moonrepo Toolchain - React Vite]] — React Vite + Vitest.
* **JavaScript/Node:** [[Moonrepo Toolchain - SvelteKit]] — SvelteKit (not the JS default path).
* **JavaScript/Node:** [[Moonrepo Toolchain - Bun]] — Bun runtime (not the JS default path).
* **Mobile:** [[Moonrepo Toolchain - Dart Flutter]] — Flutter via proto; moon has no Dart toolchain.
* **Backend:** [[Moonrepo Toolchain - Go]] — Go modules and tasks.
* **Systems:** [[Moonrepo Toolchain - Rust]] — Cargo targets.

---

### 2. Project Organization
How apps, APIs, and libraries are bounded in the workspace graph (`layer` / `language` / `stack` on each `moon.yml`).

* [Projects](https://moonrepo.dev/docs/concepts/projects) — what a project is.
* [Project config](https://moonrepo.dev/docs/config/project) — `layer`, `language`, `stack`, `dependsOn`, `project` metadata (classification / scopes).
* Task-level edges (start API with a SPA): [[Moonrepo Task Dependencies]].

---

### 3. Task Orchestration
The pipeline engine for local operations, caching, and concurrent tasks (`moon run <project>:<task>`).

* [Targets](https://moonrepo.dev/docs/concepts/target) — `project:task` (e.g. `dashboard:dev`).
* [File groups](https://moonrepo.dev/docs/concepts/file-group) — reusable globs (`@group(sources)`).
* [Tokens](https://moonrepo.dev/docs/concepts/token) — `$projectRoot`, `@in()`, `@group()`, etc.
* [Task inheritance](https://moonrepo.dev/docs/concepts/task-inheritance) — `.moon/tasks/**` + `inheritedBy` (filename does not filter).
* [[Moonrepo Task Dependencies]] — `dependsOn` vs task `deps`, where to put them, persistent watchers.

---

### 4. Global Code Quality & Linting
Monorepo-wide compliance. **JS default is Biome.** ESLint/Prettier notes are for repos that are not on Biome.

* [[Moonrepo Toolchain - TypeScript Config]] — Base `tsconfig` inheritance and `typecheck` (`tsc --noEmit`).
* [[Moonrepo Toolchain - Biome]] — Inherited `lint` / `format`.
* [[Moonrepo Toolchain - ESLint]] — ESLint tasks (not the JS default).
* [[Moonrepo Toolchain - Prettier]] — Prettier tasks (not the JS default).

---

## ❓ Open Questions
This section tracks active research gaps.

- Proto vs system Node/pnpm: [proto](https://moonrepo.dev/docs/proto) and [toolchain `versionFromPrototools`](https://moonrepo.dev/docs/config/toolchain). This vault pins tools in `.prototools` and uses empty `node: {}` / `pnpm: {}` to enable those toolchains.

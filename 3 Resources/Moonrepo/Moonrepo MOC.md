---
created: 2026-06-23 23:43
updated: 2026-06-23 23:43
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

 ---
## Getting Started & Setup
These blueprints are strictly tool-agnostic. You execute these *once* to provision a system and initialize a blank repository container.

- **System Provisioning:** [[Moonrepo Installation and Global Setup]] — Local machine CLI environment setup using `proto`.
* **Agnostic Foundation:** [[Moonrepo Workspace Initialization]] — Laying down the base container and global workspace boundaries.

### ⚡ Automation Scripts
* [[New Moonrepo Workspace Script]] — Dynamic Templater runbook to instantly output execution-ready shell setups.

---

## 📦 Core Pillars & Concepts

### 1. Workspace & Toolchain
The global root environment that manages cross-project configurations, shared languages, and environment-wide constraints.
* [[Moonrepo Workspace Definition and Structure]] — Understanding the global root `.moon/` settings directory.
* [[Moonrepo Toolchain and Language Management]] — Overriding, locking, and enforcing runtime versions across development engines via `proto`.

#### 🔌 Language & Package Integration Recipes
These modular notes track how specific language runtimes and package ecosystems plug into moonrepo's core compiler path:
* **JavaScript/Node Environment:** [[Moonrepo Toolchain - PNPM]] — Layering node package topologies across the workspace.
* **JavaScript/Node Environment:** [[Moonrepo Toolchain - Node TypeScript]] — Enforcing Node environments and build flags.
* **Mobile Environment:** [[Moonrepo Toolchain - Dart Flutter]] — Onboarding native Flutter applications into the project framework.
* **Backend Environment:** [[Moonrepo Toolchain - Go]] — Orchestrating Go build targets and fast compilation tooling.
* **Systems Environment:** [[Moonrepo Toolchain - Rust]] — Managing cargo target caching and workspace binary outputs.

---

### 2. Project Organization
How individual applications, backend APIs, and shared internal utilities are structurally mapped, bounded, and typed within the workspace graph.
* [[Moonrepo Project Classification and Scopes]] — Defining project types (`app` vs `library`) and setting folder criteria.
* [[Moonrepo Project Graph and Dependencies]] — Visualizing internal dependency graphs and mapping topological relationships.

---

### 3. Task Orchestration
The pipeline engine responsible for executing local operations, tracking changes, and building projects concurrently.
* [[Moonrepo Targets Map Scopes to Tasks]] — Understanding how specific project boundaries map tightly to execution tasks (e.g., `@project:task`).
* [[Moonrepo File Groups Cluster Project Paths]] — Grouping similar file patterns or folder globs within an independent project domain.
* [[Moonrepo Tokens for Dynamic Arguments]] — Utilizing path and variable function placeholders directly inside task arrays.

---

### 4. Global Code Quality & Linting
This pillar tracks monorepo-wide code compliance, style enforcement, and compiler strictness rules that are configured centrally and inherited automatically by child projects.

* **Core Concepts:**
    - [[Moonrepo Task Inheritance Mechanisms]] — How root-level validation tasks automatically cascade down to apps and packages.

* **Tool Integration Recipes (The "How to wire them into Moon" notes):**
    - [[Moonrepo Toolchain - ESLint Integration]] — Designing global linting streams inside `.moon/tasks/` templates.
    - [[Moonrepo Toolchain - Prettier Integration]] — Orchestrating formatting validation tasks across all source directories.
    - [[Moonrepo Toolchain - TypeScript Config Sharing]] — Managing base `tsconfig.json` inheritance patterns across shared internal packages.

---

## ❓ Open Questions
This section tracks active research gaps and framework evaluations using the `Q -` tracking convention.

- * [[Q - Should I use moonrepo toolchain version manager or stick to system tools]]
---
created: 2026-06-29 23:10
updated: 2026-09-10 23:25
tags:
  - notes
---

## Setup Workspace

[Moon Docs Reference](https://moonrepo.dev/docs/install)

Create and navigate to the workspace folder:
```bash
mkdir {{repo_name}} && cd {{repo_name}}
```

* Install moon via proto (this will create a .prototools file): 
```bash
proto install moon --pin
```

* Verify `.prototools` now shows a pinned version e.g.:
```toml
moon = <version> # i.e. "2.3.5"
```

* Install remaining pinned tools:
```bash
proto use
```

* Init Workspace:
```bash
moon init
```

* Configure `.moon/workspace.yml`:
```yaml
projects: [
  "apps/*",
  "packages/*"
]

vcs:
  client: 'git'
  defaultBranch: 'main'
```

* After `moon init`, check that `.moon/toolchains.yml` exists (plural filename; v1 used a singular name). `moon init` does **not** write the JS stack. For a pnpm + TypeScript workspace it should look like this (versions stay in `.prototools`):

```yaml
javascript:
  packageManager: 'pnpm'
# Versions: .prototools (moon versionFromPrototools defaults to true)
node: {}
pnpm: {}
typescript:
  createMissingConfig: true
  routeOutDirToCache: true
  syncProjectReferences: true
```

Empty `node: {}` / `pnpm: {}` enable those toolchains. Rust / Go / Bun notes keep their own language keys in this same file.

## Git

* Git: Verify `git init` ran. If not, run it.
	* update `.gitignore`
		* i.e. `node_modules`
```
# moon
.moon/cache
.moon/docker

# node
node_modules/
.pnpm-store/

# typescript
lib/
dist/
*.tsbuildinfo

```
## 🔧 Toolchain Setup

JS default (pnpm + TypeScript, Vite SPA and/or Node API):

* [[Moonrepo Toolchain - PNPM]]
* [[Moonrepo Toolchain - Node TypeScript]]
* [[Moonrepo Toolchain - React Vite]]

Other stacks (not the JS default):

* [[Moonrepo Toolchain - SvelteKit]]
* [[Moonrepo Toolchain - Bun]]
* [[Moonrepo Toolchain - Rust]]
* [[Moonrepo Toolchain - Go]]
* [[Moonrepo Toolchain - Dart Flutter]]

## 🧹 Code Quality Setup

JS default is **Biome** as the linter and formatter. Do not also install ESLint and Prettier on the same JS repo.

* [[Moonrepo Toolchain - TypeScript Config]]
* [[Moonrepo Toolchain - Biome]]

> Not the default JS path: [[Moonrepo Toolchain - ESLint]] and [[Moonrepo Toolchain - Prettier]] — use these only if the repo is not on Biome (legacy JS, or a non-JS stack that already uses them).

## ✅ Verification

* Run `moon check --all` to validate all projects
* Run `moon project-graph` to visualise dependency graph

## 📝 Notes

* proto manages all tooling versions — avoid installing Node/pnpm globally outside proto
* `.prototools` must be created before running `proto install moon` or `moon init`
* `pnpm-workspace.yaml` globs must match the `projects` glob in `.moon/workspace.yml`
* Each app/package needs its own `moon.yml` for moon to recognise it
* On that `moon.yml`, set `layer`, `language`, and `stack` (singular). Not `type: application`. Not `stacks:`. Without those three, inherited task files will not match even if TypeScript is detected
* Empty `inheritedBy` in moon v2 = **all** projects inherit that task file. Filters live on the task file (`inheritedBy.languages` / `stacks` / `layers`), not on the filename
* Run Code Quality templates before Toolchain templates — projects depend on shared TS/lint config
* Next: [[Moonrepo Toolchain - TypeScript Config]], [[Moonrepo Toolchain - Biome]], [[Moonrepo Task Dependencies]]

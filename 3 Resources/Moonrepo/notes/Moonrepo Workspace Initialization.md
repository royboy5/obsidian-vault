---
created: 2026-06-29 23:10
updated: 2026-06-29 23:10
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

Run the relevant toolchain template(s) for your stack:

* [[Moonrepo Toolchain - PNPM]]
* [[Moonrepo Toolchain - Node TypeScript]]
* [[Moonrepo Toolchain - React Vite]]
* [[Moonrepo Toolchain - SvelteKit]]
* [[Moonrepo Toolchain - Bun]]
* [[Moonrepo Toolchain - Rust]]
* [[Moonrepo Toolchain - Go]]
* [[Moonrepo Toolchain - Dart Flutter]]

## 🧹 Code Quality Setup

Run these for every repo — they apply monorepo-wide:

* [[Moonrepo Toolchain - TypeScript Config]]
* [[Moonrepo Toolchain - Biome]]
* [[Moonrepo Toolchain - ESLint]]
* [[Moonrepo Toolchain - Prettier]]

## ✅ Verification

* Run `moon check --all` to validate all projects
* Run `moon project-graph` to visualise dependency graph

## 📝 Notes

* proto manages all tooling versions — avoid installing Node/pnpm globally outside proto
* `.prototools` must be created before running `proto install moon` or `moon init`
* `pnpm-workspace.yaml` globs must match the `projects` glob in `.moon/workspace.yml`
* Each app/package needs its own `moon.yml` for moon to recognise it
* TypeScript Config, ESLint and Prettier tasks in `.moon/tasks/` are inherited automatically by all projects
* Run Code Quality templates before Toolchain templates — projects depend on shared TS/lint config
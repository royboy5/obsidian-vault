---
created: 2026-07-06 23:27
updated: 2026-07-06 23:27
tags:
  - notes
---
## Moonrepo PNPM Workspace Setup

* Update `.prototools` at the workspace root:
```toml
node = "26.x"
pnpm = "11.x"
```

* Install new pinned tools:
```bash
proto use
```

* Open `.moon/toolchains.yml`. For a pnpm + TypeScript workspace (`packageManager` under `javascript`, not `node`; versions in `.prototools`):

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

- Create `pnpm-workspace.yaml` at the workspace root:
```yaml
packages:
  - 'apps/*'
  - 'packages/*'
allowBuilds:
  esbuild: true
```

*  Initialize `package.json` on repo root:
```bash
pnpm init
```

- Updated `package.json`

```json
{
  "name": "@<org>/root",
  "version": "0.0.0",
  "private": true
}
```

`engines` and `package.json#packageManager` are optional. Prefer `.prototools` as the single pin. Do not also set `version:` under `node` / `pnpm` in `.moon/toolchains.yml`.

* Install dependencies:
```bash
pnpm install
```

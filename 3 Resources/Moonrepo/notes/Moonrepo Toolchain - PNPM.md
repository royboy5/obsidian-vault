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

- Create `pnpm-workspace.yaml` at the workspace root:
```yaml
packages:
  - 'apps/*'
  - 'packages/*'
```

* Create root `package.json` at the workspace root:
```json
{
  "name": "@<org>/root",
  "version": "0.0.0",
  "private": true,
  "engines": {
    "node": ">=26.0.0",
    "pnpm": ">=11.0.0"
  }
}
```

* Install dependencies:
```bash
pnpm install
```

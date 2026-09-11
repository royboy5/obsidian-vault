## 🌙 Moonrepo Toolchain: Node TypeScript

[Moon Node.js Handbook](https://moonrepo.dev/docs/guides/javascript/node-handbook)

tsdown is the production compiler for the API (same job Vite has on an SPA). `tsc` typechecks with `noEmit`. Biome lints and formats. `tsx watch` runs locally. Moon runs the tasks.

Do **not** use `tsc` to fill `dist/`. Do not add `tsconfig.build.json`. Do not install tsdown or tsx on a Vite app — see [[Moonrepo Toolchain - React Vite]].

When done:

| Job | Tool | Moon task |
|-----|------|-----------|
| Types | `tsc --pretty --noEmit` | `:typecheck` |
| Lint (CI) | `biome check .` | `:lint` |
| Format (local) | `biome check --write .` | `:format` |
| Dev | `tsx watch src/index.ts` | `:dev` |
| `dist/` | `tsdown` | `:build` |
| Run the bundle | `node dist/index.js` | `:start` |

Example id below is `backend` (`@<org>/backend`, `moon run backend:dev`).

## Prerequisites

- `.prototools` has node and pnpm (versions live here, not in `toolchains.yml`):

```toml
node = "26.8.1"
pnpm = "11.25.0"
```

- Install them:

```bash
proto use
```

- Moon workspace and pnpm workspace are already set up (`apps/*`, `packages/*`). See [[Moonrepo Workspace Initialization]] if not.
- Shared tsconfig + inherited `:typecheck` exist. See [[Moonrepo Toolchain - TypeScript Config]] if not.
- Put the app under `apps/<id>`. Do not scaffold at the workspace root.
- `@types/node` lives on the **workspace root** (one Node types version for the repo):

```bash
pnpm add -Dw @types/node
```

## Tasks Setup

Create `.moon/tasks/node-backend.yml`. Skip if it already has `dev` / `build` / `start`.

```yaml
# Server-side Node TypeScript. Vite SPAs use stack: frontend and must not inherit these.
inheritedBy:
  languages: 'typescript'
  stacks: 'backend'
  layers: 'application'

tasks:
  dev:
    command: 'tsx watch src/index.ts'
    options:
      persistent: true
  build:
    command: 'tsdown'
    inputs:
      - 'src/**/*'
      - 'tsdown.config.ts'
      - 'tsconfig.json'
      - '/tsconfig.options.json'
    outputs:
      - 'dist'
  start:
    command: 'node dist/index.js'
    deps:
      - 'build'
    options:
      persistent: false
```

- Filename does not filter. `inheritedBy` does (all three AND).
- `layers: 'application'` (singular), not `'applications'`.
- `'/tsconfig.options.json'` — leading slash is the workspace root.
- Do not copy this file into the app. Do not put these tasks on `apps/<id>/moon.yml` unless adding `deps`.
- `dev` must be `options.persistent: true`. `tsx watch` never exits. Without that, a SPA `tasks.dev.deps: ['backend:dev']` waits forever and Vite never starts. See [[Moonrepo Task Dependencies]].

## 🔧 Toolchain Setup

Open `.moon/toolchains.yml`. Check that it has top-level `javascript`, `node`, `pnpm`, and `typescript`, that `packageManager` is under `javascript` (not `node`), and that there is no `version:` under `node` or `pnpm`.

If that already matches, leave it. A Node API does not need a new toolchain.

If the file is missing, still v1-shaped, or missing any of those keys, it should look like this:

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

- Empty `node: {}` / `pnpm: {}` enable those toolchains. Pins stay in `.prototools`.
- There is no `tsdown:` toolchain. tsx, tsdown, and Biome are npm packages; commands come from `.moon/tasks/*.yml`.
- `typescript:` manages tsconfigs. It does not create `:typecheck` (that is `.moon/tasks/typescript.yml`).
- Do not copy `toolchains:` onto the app `moon.yml`.

## 📁 Project Setup

### Scaffold

```bash
mkdir -p apps/backend
cd apps/backend
pnpm init
```

- Folder name is the moon project id (`moon run backend:dev`).

### moon.yml

Create `apps/<id>/moon.yml`. Required for inheritance — moon can detect TypeScript from files, but task files match these fields.

```yaml
layer: 'application'
language: 'typescript'
stack: 'backend'

project:
  title: 'backend'
  description: 'HTTP API and server-side TypeScript application.'
```

- `stack` (singular), not `stacks`.
- `stack: 'backend'` so this app inherits tsdown, not Vite.
- No `tasks:` block. Extra `deps` (start this API with a SPA) go on the **SPA** `moon.yml` — see [[Moonrepo Task Dependencies]].

### package.json

```json
{
  "name": "@<org>/backend",
  "version": "0.0.0",
  "private": true,
  "type": "module"
}
```

- Delete `scripts` (moon owns `dev` / `build` / `start` / `typecheck` / `lint`).
- Delete `typescript` from this package. Root already has TypeScript.
- Delete `@types/node` from this package. Root already has it.
- `"type": "module"` so `node dist/index.js` is ESM.

From the repo root:

```bash
pnpm add -D tsx tsdown --filter @<org>/backend
```

- `--filter` matches `"name"`. Drop `--filter` if you are already in the app directory. Both stay on this package, not the workspace root.
- Confirm Vite apps have **no** `tsdown` or `tsx`.

### tsdown

`apps/<id>/tsdown.config.ts`:

```ts
import { defineConfig } from "tsdown";

export default defineConfig({
  entry: ["src/index.ts"],
  platform: "node",
  format: "esm",
  dts: false,
  clean: true,
  outDir: "dist",
  // package.json has "type": "module", so emit dist/index.js (not .mjs)
  fixedExtension: false,
});
```

- Comma after `false`, not a semicolon.
- Do not include this file in the app `tsconfig.json` `include` (tsdown’s published types fail under TypeScript 7). tsdown still runs it.
- Do not set `outDir` to the moon type cache. `dist/` is the k8s/Docker artifact.

### TypeScript

`apps/<id>/tsconfig.json`:

```json
{
  "extends": "../../tsconfig.options.json",
  "compilerOptions": {
    "noEmit": true,
    "moduleResolution": "bundler"
  },
  "include": ["src/**/*", "tests/**/*"],
  "references": []
}
```

- Extend `tsconfig.options.json`. Do not copy `strict` / `composite` / `skipLibCheck` onto this file.
- Do **not** put `jsx` or DOM `lib` here (and never on `tsconfig.options.json`).
- `include` is `src` and `tests`. Do **not** include `tsdown.config.ts`.

After `moon run <id>:typecheck`, moon may insert `"outDir": "../../.moon/cache/types/apps/<id>"`. Leave it. Keep `noEmit: true`. That cache is not `dist/` (`tsdown` writes `dist/`).

Root `tsconfig.json` is the solution file (`files: []` + `references`). If moon did not add `{ "path": "./apps/<id>" }`, add it.

Shared tsconfig: [[Moonrepo Toolchain - TypeScript Config]].

### Source

`apps/<id>/src/index.ts` — swap for the real entrypoint. A stub is enough to verify the graph:

```ts
console.log("moonie backend");
```

### Biome

No `apps/<id>/biome.json`. Root `biome.json` applies. See [[Moonrepo Toolchain - Biome]].

## Verify

```bash
moon project backend
```

Expect `Stack: backend`, `Layer: application`, `Language: typescript`. Inherits `.moon/tasks/node-backend.yml`, `.moon/tasks/typescript.yml`, `.moon/tasks/biome.yml`. `build` is `tsdown`. `dev` is `tsx watch`. Toolchains: `javascript, node, pnpm, typescript`.

```bash
moon project dashboard
```

If a Vite app exists, `build` must still be `vite build`. If this API’s `build` is `vite build`, `stack` is wrong — do not `inheritedTasks.exclude`.

```bash
moon run backend:typecheck
moon run backend:lint
moon run backend:build
```

`apps/backend/dist/` should contain `index.js`, not an SPA `index.html`.

## Later APIs

1. New folder `apps/<new-id>`
2. Same `moon.yml` fields (`stack: 'backend'`)
3. Repeat package.json, tsdown config, tsconfig, `src/index.ts`
4. Do not copy `node-backend.yml`

## 📝 Notes

- Do not create Node APIs at the workspace root — always `apps/` (or `packages/` for libraries).
- Tasks are inherited via `language` / `stack` / `layer`. Do not copy `dev` / `build` / `start` into the app.
- `private: true` prevents accidental npm publish.
- Name packages `@<org>/<id>` — e.g. `@huddle-up/identity-api`.
- Do not use Vite for a Node API. Do not use tsdown for a browser app.
- Run [[Moonrepo Toolchain - TypeScript Config]] if shared tsconfigs are not set up yet.
- Run [[Moonrepo Toolchain - Biome]] if lint/format tasks are not set up yet.
- SPA `deps` to start this API: [[Moonrepo Task Dependencies]].

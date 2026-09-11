## 🌙 Moonrepo Toolchain: React (Vite + Vitest)

[Moon Vite & Vitest Docs](https://moonrepo.dev/docs/guides/examples/vite)

Vite is the production compiler for the SPA (same job tsdown has on a Node backend). `tsc` typechecks with `noEmit`. Biome lints and formats. Vitest tests. Moon runs the tasks.

Stock `pnpm create vite` is a standalone repo. Keep the React/Vite source and React Compiler plugins. Replace scripts, TypeScript, Oxlint, and the tsconfig trio with the workspace.

When done:

| Job | Tool | Moon task |
|-----|------|-----------|
| Types | `tsc --pretty --noEmit` | `:typecheck` |
| Lint (CI) | `biome check .` | `:lint` |
| Format (local) | `biome check --write .` | `:format` |
| Dev | `vite` | `:dev` |
| `dist/` | `vite build` | `:build` |
| Preview | `vite preview` | `:preview` |
| Tests | `vitest run` | `:test` |

Example id below is `dashboard` (`@<org>/dashboard`, `moon run dashboard:dev`).

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
- Put the app under `apps/<id>`. Do not scaffold at the workspace root.

## Tasks Setup

Create `.moon/tasks/vite-frontend.yml`. Skip if it already has `dev` / `build` / `preview` / `test`. If it exists but has no `test`, add only that block.

```yaml
# Browser SPAs. Node APIs use stack: backend and must not inherit these tasks.
inheritedBy:
  languages: 'typescript'
  stacks: 'frontend'
  layers: 'application'

tasks:
  dev:
    command: 'vite'
    options:
      persistent: true
  build:
    command: 'vite build'
    inputs:
      - 'src/**/*'
      - 'index.html'
      - 'vite.config.ts'
      - 'tsconfig.json'
      - '/tsconfig.options.json'
      - 'package.json'
    outputs:
      - 'dist'
  preview:
    command: 'vite preview'
    deps:
      - 'build'
    options:
      persistent: true
      runInCI: false
  test:
    command: 'vitest run'
    inputs:
      - 'src/**/*'
      - 'tests/**/*'
      - 'vite.config.ts'
      - 'tsconfig.json'
      - '/tsconfig.options.json'
      - 'package.json'
```

- Filename does not filter. `inheritedBy` does (all three AND).
- Do not copy this file into the app. Do not put these tasks on `apps/<id>/moon.yml`.
- Do not add `start: node dist/index.js`. Vite `dist/` is HTML and assets.
- `test` must be `vitest run` (one shot). Watch (`vitest`) would hang `moon ci`.

## 🔧 Toolchain Setup

Open `.moon/toolchains.yml`. Check that it has top-level `javascript`, `node`, `pnpm`, and `typescript`, that `packageManager` is under `javascript` (not `node`), and that there is no `version:` under `node` or `pnpm`.

If that already matches, leave it. A Vite app does not need a new toolchain.

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
- There is no `vite:` toolchain. Vite, Vitest, and Biome are npm packages; commands come from `.moon/tasks/*.yml`.
- `typescript:` manages tsconfigs (`outDir` cache, project references). It does not create `:typecheck` (that is `.moon/tasks/typescript.yml`).
- Do not copy `toolchains:` onto the app `moon.yml`.

## 📁 Project Setup

### Scaffold

From the workspace root (`CI=true` if pnpm prompts to purge `node_modules` with no TTY):

```bash
cd apps
CI=true pnpm create vite dashboard --template react-compiler-ts
```

- Folder name is the moon project id (`moon run dashboard:dev`).
- `react-compiler-ts` = TypeScript + React Compiler. `react-ts` is the same without the compiler.

Create Vite typically adds:

- Keep: `index.html`, `src/`, `public/`, `vite.config.ts`, React, Vite, `@vitejs/plugin-react`, compiler packages
- Strip later: `scripts`, app-level `typescript`, `tsconfig.app.json`, `tsconfig.node.json`, Oxlint if present, nested README commands

### moon.yml

Create `apps/<id>/moon.yml`. Required for inheritance — moon can detect TypeScript from files, but task files match these fields.

```yaml
layer: 'application'
language: 'typescript'
stack: 'frontend'

project:
  title: 'dashboard'
  description: 'React Vite frontend application.'
```

- `stack` (singular), not `stacks`.
- `stack: 'frontend'` so this app inherits Vite, not tsdown.
- No `tasks:` block.

### package.json

```json
{
  "name": "@<org>/dashboard",
  "version": "0.0.0",
  "private": true,
  "type": "module"
}
```

- Delete `scripts` (moon owns `dev` / `build` / `lint` / `preview` / `test`). Leaving `"build": "tsc -b && vite build"` fights `:typecheck`.
- Delete `typescript` from this package. Root already has TypeScript.
- Delete Oxlint if present (`oxlint`, `.oxlintrc.json`). Use [[Moonrepo Toolchain - Biome]] at the workspace root.
- Delete `@types/node` from this package. Root already has it.
- Keep `react`, `react-dom`, `vite`, `@vitejs/plugin-react`, `@types/react`, `@types/react-dom`.
- Keep compiler packages if you used `react-compiler-ts` (`babel-plugin-react-compiler`, `@rolldown/plugin-babel`, `@babel/core`, `@types/babel__core`).

From the repo root:

```bash
CI=true pnpm install
CI=true pnpm add -D vitest jsdom --filter @<org>/dashboard
```

- `--filter` matches `"name"`. Drop `--filter` if you are already in the app directory. Vitest 4 does not auto-install `jsdom`. Both stay on this package, not the workspace root.
- Confirm there is **no** `tsdown` or `tsx` on this package.

### TypeScript

One app tsconfig. Delete `tsconfig.app.json` and `tsconfig.node.json`.

`apps/<id>/tsconfig.json`:

```json
{
  "extends": "../../tsconfig.options.json",
  "compilerOptions": {
    "noEmit": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "jsx": "react-jsx",
    "lib": ["ES2022", "DOM", "DOM.Iterable"]
  },
  "include": ["src/**/*", "tests/**/*"],
  "references": []
}
```

- Extend `tsconfig.options.json`. Do not copy `strict` / `composite` / `skipLibCheck` onto this file.
- `jsx` and DOM `lib` only here — not on `tsconfig.options.json` (that would hit Node apps).
- `include` is `src` and `tests`. Do **not** include `vite.config.ts` (Vite’s published types fail under TypeScript 7; same reason a Node app omits `tsdown.config.ts`). Vite still runs that file.
- `allowImportingTsExtensions: true` is required because Create Vite imports `./App.tsx` (extension in the path). TypeScript only allows that with this flag, and only when `noEmit` (or `emitDeclarationOnly`) is set. Leave the Create Vite imports as they are.

`apps/<id>/src/vite-env.d.ts` (create if Create Vite did not):

```ts
/// <reference types="vite/client" />
```

- Create Vite often put `"types": ["vite/client"]` on `tsconfig.app.json`. Do not copy that onto this tsconfig — `compilerOptions.types` is an allowlist and will hide other `@types`.
- Tests `import { describe, it, expect } from "vitest"`. Types ship with the package. Do not add `"types": ["vitest/globals"]` unless you set `test.globals: true` (this guide does not).

After `moon run <id>:typecheck`, moon may insert `"outDir": "../../.moon/cache/types/apps/<id>"`. Leave it. Keep `noEmit: true`. That cache is not `dist/` (`vite build` writes `dist/`).

Root `tsconfig.json` is the solution file (`files: []` + `references`). If moon did not add `{ "path": "./apps/<id>" }`, add it.

Shared tsconfig: [[Moonrepo Toolchain - TypeScript Config]].

### Vite config

Keep `vite.config.ts` and `index.html` at the app root (`<script type="module" src="/src/main.tsx">`).

```ts
import react, { reactCompilerPreset } from "@vitejs/plugin-react";
import babel from "@rolldown/plugin-babel";
import { defineConfig } from "vitest/config";

export default defineConfig({
  plugins: [react(), babel({ presets: [reactCompilerPreset()] })],
  test: {
    environment: "jsdom",
  },
});
```

- `defineConfig` from `vitest/config` types the `test` key. Do not add a second `vitest.config.ts` (Vitest would ignore `vite.config.ts`).
- `react-ts` template: `plugins: [react()]` only.
- Do not set `build.outDir` to the moon type cache. Default `dist/` is already gitignored.

### Tests

Create `apps/<id>/tests/smoke.test.ts`. `vitest run` with no files fails, so `moon check` fails.

```ts
import { describe, expect, it } from "vitest";

describe("smoke", () => {
  it("runs vitest", () => {
    expect(true).toBe(true);
  });
});
```

- Put tests in `tests/` (matches Biome / typecheck inputs). Colocated `src/**/*.test.tsx` also runs.
- `@testing-library/react` is optional and not required for moon.

### Biome

No `apps/<id>/biome.json`. Root `biome.json` applies (`recommended` rules). See [[Moonrepo Toolchain - Biome]].

Open root `biome.json`. Check `files.includes` has `"**"` and `"!**/public"`. If `includes` is missing, add it (keep `ignoreUnknown`):

```json
"files": {
	"ignoreUnknown": false,
	"includes": ["**", "!**/public"]
}
```

`!**/public` skips Create Vite favicons/sprites (`lint/a11y/noSvgWithoutTitle`). Those files are static assets, not in the a11y tree. Do not add a per-app `biome.json` to silence that rule. `dist/` is already skipped via `vcs.useIgnoreFile` and `.gitignore`.

Format first, then lint:

```bash
moon run <id>:format
moon run <id>:lint
```

Create Vite uses single quotes and 2-space indent; `:lint` is check-only and will fail until format runs.

`:format` / `:lint` both run `biome check`, so **lint errors also fail format**. The stock template also trips:

| File | Rule | Fix |
|------|------|-----|
| `src/main.tsx` | `lint/style/noNonNullAssertion` | `getElementById`, throw if missing, then `createRoot(root)` — no `!` |
| `src/App.tsx` | `lint/a11y/noAmbiguousAnchorText` | Change “Learn more” to something specific, e.g. “Learn more about React” |

### Gitignore

Repo `.gitignore` already has `node_modules/`, `dist/`, `*.tsbuildinfo`, `.moon/cache`. Nested `apps/<id>/.gitignore` is optional; it must not un-ignore `dist/`. Delete Create Vite’s README if it tells people to run `pnpm dev` / Oxlint.

## Verify

```bash
moon project dashboard
```

Expect `Stack: frontend`, `Layer: application`, `Language: typescript`. Inherits `.moon/tasks/vite-frontend.yml`, `.moon/tasks/typescript.yml`, `.moon/tasks/biome.yml`. `build` is `vite build`. `test` is `vitest run`. Toolchains: `javascript, node, pnpm, typescript`.

```bash
moon project backend
```

If a backend exists, `build` must still be `tsdown`. If the new app’s `build` is tsdown, `stack` is wrong — do not `inheritedTasks.exclude`.

```bash
moon run dashboard:typecheck
moon run dashboard:lint
moon run dashboard:test
moon run dashboard:build
```

`apps/dashboard/dist/` should contain `index.html` and assets, not a Node `index.js`.

## Later SPAs

1. `pnpm create vite` into `apps/<new-id> --template react-compiler-ts`
2. Same `moon.yml` fields
3. Repeat package.json, tsconfig, Vite config, smoke test, format
4. Do not copy `vite-frontend.yml`. If `test` is missing there, add it once, then add `vitest` + `jsdom` to every frontend app that inherits it

A shared UI package is `layer: library`, `stack: frontend`, declarations on **that** tsconfig (`emitDeclarationOnly`). Apps stay `noEmit`. See [[Moonrepo Toolchain - TypeScript Config]].

## 📝 Notes

- Do not create Vite projects at the workspace root — always `apps/` (or `packages/` for libraries).
- Tasks are inherited via `language` / `stack` / `layer`. Do not copy `dev` / `build` into the app. Extra `deps` (start API with the SPA) go on the app `moon.yml` — see [[Moonrepo Task Dependencies]].
- `private: true` prevents accidental npm publish.
- Name packages `@<org>/<id>` — e.g. `@huddle-up/web`.
- Do not use tsdown for a browser app.
- Do not put `jsx` on root `tsconfig.options.json`.
- Do not add a second `biome.json`.
- Run [[Moonrepo Toolchain - TypeScript Config]] if shared tsconfigs are not set up yet.
- Run [[Moonrepo Toolchain - Biome]] if lint/format tasks are not set up yet.

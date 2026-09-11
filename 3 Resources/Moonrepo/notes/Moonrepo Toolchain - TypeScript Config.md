## 🌙 Moonrepo Toolchain: TypeScript Config

[Moon TypeScript Docs](https://moonrepo.dev/docs/guides/examples/typescript)

[Moon TS project reference](https://moonrepo.dev/docs/guides/javascript/typescript-project-refs)

Shared tsconfigs + inherited `typecheck`. Apps typecheck with `tsc --pretty --noEmit`. Bundlers emit JS (tsdown / Vite). Do not use `tsc --build` until a `packages/*` library needs project-reference builds.

Skip the pnpm workspace bits if [[Moonrepo Workspace Initialization]] and [[Moonrepo Toolchain - PNPM]] are already done.

## Tasks Setup

Create `.moon/tasks/typescript.yml`. Filename does not filter. `inheritedBy` does.

```yaml
# Matches `language: typescript` on each project's moon.yml.
# Prefer languages here. `toolchains: typescript` in this same map would
# match the `typescript:` toolchain instead of the language field.
inheritedBy:
  languages: 'typescript'

tasks:
  typecheck:
    command:
      - 'tsc'
      - '--pretty'
      - '--noEmit'
    inputs:
      - 'src/**/*'
      - 'tests/**/*'
      - 'types/**/*'
      - 'tsconfig.json'
      - 'tsconfig.*.json'
      - '/tsconfig.options.json'
```

- No `outputs:` — nothing is emitted.
- Empty `inheritedBy` would give `tsc` to every project (including Rust/Go later).
- Later, when a `packages/*` library needs `tsc --build`, change the command then. Apps stay `noEmit`.

## Toolchain Setup

Open `.moon/toolchains.yml`. Check that it has top-level `javascript`, `node`, `pnpm`, and `typescript`, that `packageManager` is under `javascript` (not `node`), and that there is no `version:` under `node` or `pnpm`. If missing or still v1-shaped:

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

`typescript:` manages tsconfigs (`outDir` cache, project references). It does not create `:typecheck`.

Install TypeScript at the workspace root if it is not there yet:

```bash
pnpm add -D -w typescript
```

## Root-level Configuration

Create `tsconfig.options.json` (shared compiler options). This is the nodenext / `strict` / `composite` / `declaration` baseline. **Do not** put `jsx` or DOM `lib` here — that would hit Node apps.

```json
{
  "compilerOptions": {
    "moduleResolution": "nodenext",
    "target": "es2022",
    "skipLibCheck": true,
    "strict": true,
    "composite": true,
    "declaration": true,
    "declarationMap": true,
    "incremental": true,
    "noEmitOnError": true
  }
}
```

Create root `tsconfig.json` (solution file). It compiles nothing. `syncProjectReferences` fills `references`.

```json
{
  "extends": "./tsconfig.options.json",
  "files": [],
  "references": []
}
```

> `extends` here is harmless but functionally inert — `"files": []` means this config never compiles anything itself. Kept for convention (matches moon's docs); safe to drop if that bothers you.

## Project-level Configuration

Each app/package needs `tsconfig.json`. Extend `tsconfig.options.json`, not the root solution file. `noEmit` + `moduleResolution: bundler`. Do **not** include bundler configs (`vite.config.ts`, `tsdown.config.ts`) — their published types fail under TypeScript 7; the bundler still runs those files.

***Node (tsdown)***

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

***Vite SPA*** — same, plus `jsx` / DOM `lib` / maybe `allowImportingTsExtensions`. Full snippet: [[Moonrepo Toolchain - React Vite]].

After `moon run <id>:typecheck`, moon may insert `"outDir": "../../.moon/cache/types/apps/<id>"` (`routeOutDirToCache`). Leave it. That cache is not `dist/`.

If moon did not add `{ "path": "./apps/<id>" }` to the root solution file, add it.

A shared UI package is `layer: library` with `emitDeclarationOnly` on **that** tsconfig. Apps stay `noEmit`.

## Internal / shared packages

For packages consumed only inside the monorepo (not published to npm), skip a build step — point `package.json` at source so consuming apps type-check and bundle it directly:

```json
{
  "name": "@<org>/shared-types",
  "private": true,
  "type": "module",
  "main": "./src/index.ts",
  "types": "./src/index.ts",
  "exports": { ".": "./src/index.ts" }
}
```

Consuming projects:

```json
{ "dependencies": { "@<org>/shared-types": "workspace:*" } }
```

> The most common breakage: a mismatch between the actual entry file extension and what `main` / `types` / `exports` point at (`.ts` vs `.tsx`). Module resolution fails with "cannot find module" if these drift.

`composite: true` on `tsconfig.options.json` is required when a project is listed in another project's `references` (TS6306). A project only co-listed under the root solution file does not need it by itself.

## 📝 Notes

* `-w` tells pnpm to install at the workspace root
* App `compilerOptions` belong on the project tsconfig (`jsx`, DOM `lib`, `bundler`). Not on `tsconfig.options.json`
* `syncProjectReferences: true` lives in `.moon/toolchains.yml` under `typescript:` — not `.moon/workspace.yml`
* Tasks in `.moon/tasks/` are inherited via `inheritedBy`, not the filename. No `typecheck` on each app `moon.yml`
* Always pass `--pretty` so colour survives moon's runner
* Add `lib/`, `dist/`, and `*.tsbuildinfo` to `.gitignore`
* Optional extra under `typescript:`: `includeProjectReferenceSources: true` (go-to-definition jumps to source instead of a `.d.ts` stub)

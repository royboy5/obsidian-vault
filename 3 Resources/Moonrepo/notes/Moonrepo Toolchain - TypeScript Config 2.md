## 🌙 Moonrepo Toolchain: TypeScript Config

[Moon TypeScript Docs](https://moonrepo.dev/docs/guides/examples/typescript)
[Moon TS project reference](https://moonrepo.dev/docs/guides/javascript/typescript-project-refs)
## 🔧 Setup

If using PNPM on multiple apps/packages, we need to create a pnpm workspace.

Initialize `package.json`
```bash
pnpm init
```

Update `package.json`:
```json
{
  "name": "@<org>/<monorepo>",
  "version": "0.0.0",
  "private": true
}
```

- Create `pnpm-workspace.yaml` at the monorepo root.
```yaml
packages:
  - 'apps/*'
  - 'packages/*'
```

* Install at the workspace root:
```bash
pnpm add -D -w typescript
```

* Create `.moon/tasks/typescript.yml`:
```yaml
tasks:
  typecheck:
    command:
      - 'tsc'
      - '--build'
      - '--pretty'
      - '--verbose'
    inputs:
      - 'src/**/*'
      - 'tests/**/*'
      - 'types/**/*'
      - 'tsconfig.json'
      - 'tsconfig.*.json'
      - '/tsconfig.options.json'
    outputs:
      - 'lib'
```

* Create `.moon/toolchains.yml`:
```yaml
typescript:
  # Auto-populates every tsconfig.json's "references" array from your
  # package.json workspace dependencies — no hand-maintaining them.
  syncProjectReferences: true
  # Cross-project "go to definition" jumps to real source instead of a
  # compiled .d.ts stub while you're developing both sides at once.
  includeProjectReferenceSources: true
```

## Root-level Configuration

* Create root `tsconfig.options.json` (shared compiler options — **universal  settings only**, not `target`/`module`/`moduleResolution`/`lib`/`types`,since those differ per project and get fully overridden, not merged, when a project sets its own):

```json
{
  "compilerOptions": {
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "isolatedModules": true,
    "resolveJsonModule": true,
    "composite": true,
    "declaration": true,
    "declarationMap": true,
    "incremental": true
  }
}
```

* Create root `tsconfig.json` (houses all project references):
```json
{
  "extends": "./tsconfig.options.json",
  "files": [],
  // All project references in the repo
  "references": []
}
```

> `extends` here is harmless but functionally inert — `"files": []` means this config never compiles anything itself, so the inherited
> `compilerOptions` never actually apply to any file. Kept for convention (matches moon's own docs); safe to drop if that bothers you.
## Project-level Configuration

* Add `tsconfig.json` to each project — this is where the axis options that  actually differ per project (Node app vs. browser app vs. plain package)  belong:

```json
{
  "extends": "../../tsconfig.options.json",
  "compilerOptions": {
    // These differ per project — Node app vs. browser app vs. plain package.
    // Node example: "module": "nodenext", "moduleResolution": "nodenext", "types": ["node"]
    // Browser example: "lib": ["es2022", "dom"], "jsx": "react-jsx", "moduleResolution": "bundler"
    "target": "es2022",
    "outDir": "lib"
  },
  "include": ["src/**/*", "tests/**/*"],
  // Depends on other projects — required if this project imports from them
  "references": []
}
```

* Optionally extend the global `typecheck` task in project `moon.yml`:

```yaml
tasks:
  typecheck:
    args:
      - '--force'
```

## Internal / shared packages

For packages consumed only inside the monorepo (not published to npm),
skip giving them a build step — point `package.json` straight at source so
consuming apps type-check and bundle it directly, no dist folder to keep in
sync:
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
Consuming projects depend on it the normal workspace way:
```json
{ "dependencies": { "@<org>/shared-types": "workspace:*" } }
```
> The most common breakage here: a mismatch between the actual entry file
> extension and what `main`/`types`/`exports` point at (`.ts` vs `.tsx`).
> Module resolution fails silently with "cannot find module" if these drift.
## 📝 Notes
* `-w` flag is required — tells pnpm to install at the workspace root, not a project
* `tsconfig.options.json` holds only settings that are truly universal across every project (`strict`, `composite`, `declaration`, etc.) — `target`/`module`/`moduleResolution`/`lib`/`types` belong at the project level, since they don't merge across `extends`, they fully replace
* `syncProjectReferences: true` lives in `.moon/toolchain.yml`, under a `typescript:` key — **not** `.moon/workspace.yml`
* `composite: true` is only *required* on a project that's actually listed in another project's own `references` array (a real dependency) — TypeScript enforces this with error `TS6306` if missing. A project only ever co-listed under the root solution file, with nothing depending on it, doesn't need it.
* Tasks in `.moon/tasks/` are inherited automatically by all projects — no need to add `typecheck` to each `moon.yml`
* Always pass `--pretty` to preserve colour output when running in moon's task runner
* Project-level `tsconfig.json` should extend `tsconfig.options.json`, not the root `tsconfig.json`
* Add `lib/` and `*.tsbuildinfo` to `.gitignore`
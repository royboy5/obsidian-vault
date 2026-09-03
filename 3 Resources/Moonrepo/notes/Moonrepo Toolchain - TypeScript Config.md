## 🌙 Moonrepo Toolchain: TypeScript Config

[Moon TypeScript Docs](https://moonrepo.dev/docs/guides/examples/typescript)

[Moon TS project reference](https://moonrepo.dev/docs/guides/javascript/typescript-project-refs)

## 🔧 Setup

If using PNPM on multiple apps/packages, we need to create a pnpm workspace. (skip if this is already done)

Initialize `package.json` 
```bash
pnpm init
```

Update `package.json`:
```json
{
  "name": "@<org>/<monorepo>",
  "version": "0.0.0",
  "private": true,
  // Optional but suggested
  "scripts": {
	  "check": "moon check --all"
  }
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
# Matches 'language: typescript' in each projects'moon.yml
# toolchains: typescript - turns on the TS plugin
# Will only be inherited by projects that has language set to typescript

inheritedBy:
  languages: typescript
  
# Build will be used by a bundler (tsdown)
tasks:
  typecheck:
    command:
      - 'tsc'
      # - '--build'
      - '--pretty'
      # - '--verbose'
      - '--noEmit'
    inputs:
      - 'src/**/*'
      - 'tests/**/*'
      - 'types/**/*'
      - 'tsconfig.json'
      - 'tsconfig.*.json'
      - '/tsconfig.options.json'
    # outputs:
      # - 'lib'
```

## Root-level Configuration

* Create root `tsconfig.options.json` (shared compiler options):
```json
{
  "compilerOptions": {
    // Your custom options
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

* Create root `tsconfig.json` (houses all project references):
```json
{
  "extends": "./tsconfig.options.json",
  "files": [],
  // All project references in the repo
  "references": [
	  // path of apps / packages
	  {
		  "path": "./apps/<appName>",
		  "path": "./packages/<packageName>"
	  }
  ]
}
```

> `extends` here is harmless but functionally inert — `"files": []` means this config never compiles anything itself, so the inherited
> `compilerOptions` never actually apply to any file. Kept for convention (matches moon's own docs); safe to drop if that bothers you.
## Project-level Configuration

* Add `tsconfig.json` to each project:

***Node***
```json
{
  // Extend the root compiler options
  "extends": "../../tsconfig.options.json",
  "compilerOptions": {
    // Declarations are written here
    "noEmit": true,
    "moduleResolution": "bundler",
    "outDir": "../../.moon/cache/types/apps/<appName>"
  },
  // Include files in the project
  "include": ["src/**/*", "tests/**/*"],
  // Depends on other projects
  "references": []
}
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
* `tsconfig.options.json` holds shared compiler options — never define `compilerOptions` directly in `tsconfig.json`
* `syncProjectReferences: true` in `.moon/toolchain.yml` keeps project references in sync automatically
* Tasks in `.moon/tasks/` are inherited automatically by all projects — no need to add `typecheck` to each `moon.yml`
* Always pass `--pretty` to preserve colour output when running in moon's task runner
* Project-level `tsconfig.json` should extend `tsconfig.options.json` not the root `tsconfig.json`
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

## Root-level Configuration

* Create root `tsconfig.options.json` (shared compiler options):
```json
{
  "compilerOptions": {
    // Your custom options
    "moduleResolution": "nodenext",
    "target": "es2022"
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

## Project-level Configuration

* Add `tsconfig.json` to each project:
```json
{
  // Extend the root compiler options
  "extends": "../../tsconfig.options.json",
  "compilerOptions": {
    // Declarations are written here
    "outDir": "lib"
  },
  // Include files in the project
  "include": ["src/**/*", "tests/**/*"],
  // Depends on other projects
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

## 📝 Notes

* `-w` flag is required — tells pnpm to install at the workspace root, not a project
* `tsconfig.options.json` holds shared compiler options — never define `compilerOptions` directly in `tsconfig.json`
* `syncProjectReferences: true` in `.moon/toolchain.yml` keeps project references in sync automatically
* Tasks in `.moon/tasks/` are inherited automatically by all projects — no need to add `typecheck` to each `moon.yml`
* Always pass `--pretty` to preserve colour output when running in moon's task runner
* Project-level `tsconfig.json` should extend `tsconfig.options.json` not the root `tsconfig.json`
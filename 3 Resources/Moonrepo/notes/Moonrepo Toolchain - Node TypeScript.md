## 🌙 Moonrepo Toolchain: Node TypeScript

[Moon Node.js Handbook](https://moonrepo.dev/docs/guides/javascript/node-handbook)

## Root-level Config

- Add `@types/node` to the root `package.json` if multiple apps/packages use typescript. We want to have a centralized version of Node across the entire repo.
```bash
pnpm add -Dw @types/node
```

## 📁 Project Setup

* Create the project folder in the appropriate location:
```bash
# For deployable applications
mkdir apps/<project>

# For shared libraries / packages
mkdir packages/<project>

cd apps/<project>  # or packages/<project>
```

* Create `moon.yml` in the project folder:
```yaml
layer: 'application'   # or 'library' for packages
language: 'typescript'
```

* Initialize `package.json` (run from inside the project folder, not the apps or packages folder):
```bash
pnpm init
```

* Update `package.json`:
```json
{
  "name": "@<org>/<project>",
  "version": "0.0.0",
  "private": true
}
```

## 🔧 Toolchain Setup

* Add to `.moon/toolchains.yml`:
```yaml
node:
  version: "26.0.0"
  packageManager: pnpm
  addEnginesConstraint: true
typescript:
  createMissingConfig: true
  routeOutDirToCache: true
  syncProjectReferences: true
```

* Add to `.prototools` (if necessary):
```toml
node = "26.x"
pnpm = "11.x"
```

- Install tools (if necessary)
```bash
proto use
```

* Install tsx at the project root:
```bash
pnpm add -D tsx
```

* Add tasks to project `moon.yml`:
```yaml
layer: 'application'
language: 'typescript'

project:
  name: 'moon'
  description: 'A repo management tool.'
  channel: '#moon'
  owner: 'infra.platform'
  maintainers: ['miles.johnson']
tasks:
  dev:
    command: pnpm tsx watch src/index.ts
  build:
    command: pnpm tsc
  start:
    command: node dist/index.js
  typecheck:
    command: pnpm tsc --noEmit
```

## 📝 Notes

* `syncProjectReferences: true` keeps tsconfig paths in sync across packages automatically
* `tsx` is installed per project — only Node TypeScript backends need it
* `private: true` in `package.json` prevents accidental publishing to npm
* Swap `src/index.ts` for your actual entrypoint
* Use `@<org>/<project>` naming for all projects — e.g. `@huddle-up/identity-api`
* Run `Moonrepo Toolchain - TypeScript Config` to set up shared tsconfig files
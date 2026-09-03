## 🌙 Moonrepo Toolchain: React (Vite + Vitest)

[Moon Vite & Vitest Docs](https://moonrepo.dev/docs/guides/examples/vite)

## 📁 Project Setup

* Create the project folder in the appropriate location:
```bash
# For deployable applications
mkdir apps/<project>

# For shared libraries / packages
mkdir packages/<project>

cd apps/<project>  # or packages/<project>
```

* Create a new Vite project (do not run at workspace root):
```bash
pnpm create vite
```

* Update `package.json`:
```json
{
  "name": "@<org>/<project>",
  "version": "0.0.0",
  "private": true
}
```

* Create `moon.yml` in the project folder:
```yaml
language: typescript
layer: application  # or 'library' for packages
```

## 🔧 Toolchain Setup

* Add to `.moon/toolchain.yml`:
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

* Install Vitest:
```bash
pnpm add -D vitest
```

* Add `vite.config.ts` to the project root:
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  build: {
    outDir: 'dist',
  },
  test: {
    // vitest settings here
  },
})
```

* Add tags to project `moon.yml` using official presets (recommended):
```yaml
language: typescript
type: application  # or 'library' for packages
tags: ['vite', 'vitest']
```

## 📝 Notes

* Do not create Vite projects at the workspace root — always inside `apps/` or `packages/`
* Tasks are inherited via moon preset tags — no need to define them manually
* `private: true` in `package.json` prevents accidental publishing to npm
* Use `@<org>/<project>` naming for all projects — e.g. `@huddle-up/web`
* Run `Moonrepo Toolchain - TypeScript Config` to set up shared tsconfig files
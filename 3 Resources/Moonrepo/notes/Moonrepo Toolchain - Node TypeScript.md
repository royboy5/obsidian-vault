## 🌙 Moonrepo Toolchain: Node TypeScript

[Moon Node.js Handbook](https://moonrepo.dev/docs/guides/javascript/node-handbook)

## Root-level Config

- Add `@types/node` to the root `package.json` if multiple apps/packages use typescript. We want to have a centralized version of Node across the entire repo.

```bash
pnpm add -Dw @types/node
```

- (Optional) Can update root `package.json`
## 🔧 Toolchain Setup - Enable the language

* Add to `.prototools` (if necessary):
```toml
node = "26.x"
pnpm = "11.x"
```

- Install tools (if necessary)
```bash
proto use
```

* Add to `.moon/toolchains.yml`:
```yaml
# Enable JavaScript
javascript:
  packageManager: 'pnpm'

# Enable Node.js and pnpm
node: {}
pnpm: {}
```
*Note: versions are empty here because it will inherit the proto pin*

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


* Install tsx at the project root:
```bash
pnpm add -D tsx tsdown
```

- `tsdown.config.ts`
```ts
import { defineConfig } from 'tsdown';

export default defineConfig({
  entry: ["src/index.ts"],
  platform: "node",
  format: "esm",
  dts: false,
  clean: true,
  outDir: "dist",
  // package.json has "type: module", so emit dist/index.js (not .mjs)
  fixedExtension: false;
});
```

* Add tasks to project `moon.yml`:
```yaml
layer: 'application'
language: 'typescript'

# `backend` - Server-side APIs, etc.
# `data` - Data sources, database layers, etc. v2.0.0
# `frontend` - Client-side user interfaces, etc.
# `infrastructure` - Cloud/server infrastructure, Docker, etc.
# `systems` - Low-level systems programming.
# `unknown` (default) - When not configured.
stack: 'backend'

project:
  name: 'moon'
  description: 'A repo management tool.'
  channel: '#moon'
  owner: 'infra.platform'
  maintainers: ['miles.johnson']
tasks:
  dev:
    command: tsx watch src/index.ts
  build:
	# Bundler command here.  This will use tsdown
    command: tsdown
    inputs:
	  - src/**/*
	  - tsdown.config.ts
	  - tsconfig.json
	  - /tsconfig.options.json
	outputs:
	  - dist
  start:
    command: node dist/index.js
    deps:
      - build
	options:
	  persistent: false
```

## 📝 Notes

* `syncProjectReferences: true` keeps tsconfig paths in sync across packages automatically
* `tsx` is installed per project — only Node TypeScript backends need it
* `private: true` in `package.json` prevents accidental publishing to npm
* Swap `src/index.ts` for your actual entrypoint
* Use `@<org>/<project>` naming for all projects — e.g. `@huddle-up/identity-api`
* Run `Moonrepo Toolchain - TypeScript Config` to set up shared tsconfig files
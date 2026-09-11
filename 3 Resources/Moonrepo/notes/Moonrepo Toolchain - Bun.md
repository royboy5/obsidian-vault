## 🌙 Moonrepo Toolchain: Bun

Not the JS default path (that is pnpm + [[Moonrepo Toolchain - Node TypeScript]] / [[Moonrepo Toolchain - React Vite]]).

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
layer: 'application'
language: 'typescript'
stack: 'backend'
```

* Initialise `package.json`:
```bash
bun init
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
bun:
  version: "1.1.0"
```

* Add to `.prototools`:
```toml
bun = "1.1.0"
```

* Add tasks to project `moon.yml`:
```yaml
layer: 'application'
language: 'typescript'
stack: 'backend'
tasks:
  dev:
    command: bun run src/index.ts
  build:
    command: bun build src/index.ts --outdir dist
  test:
    command: bun test
  typecheck:
    command: bun tsc --noEmit
```

## 📝 Notes

* Bun acts as both runtime and package manager — swap out pnpm in `.prototools` and `.moon/toolchains.yml` if using Bun exclusively
* `private: true` in `package.json` prevents accidental publishing to npm
* Use `@<org>/<project>` naming for all projects — e.g. `@huddle-up/worker`
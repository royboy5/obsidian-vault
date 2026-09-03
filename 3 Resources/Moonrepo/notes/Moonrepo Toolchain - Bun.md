## 🌙 Moonrepo Toolchain: Bun

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
language: typescript
type: application  # or 'library' for packages
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

* Add to `.moon/toolchain.yml`:
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
language: typescript
type: application  # or 'library' for packages
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

* Bun acts as both runtime and package manager — swap out pnpm in `.prototools` and `toolchain.yml` if using Bun exclusively
* `private: true` in `package.json` prevents accidental publishing to npm
* Use `@<org>/<project>` naming for all projects — e.g. `@huddle-up/worker`
## 🌙 Moonrepo Toolchain: React (Vite + Vitest)

[Moon Vite & Vitest Docs](https://moonrepo.dev/docs/guides/examples/vite)

## Prerequisits

- `.prototools` 
```toml
node = "26.x"
pnpm = "11.x"
```

- Install node / pnpm
```bash
proto use
```

- moon workspace and pnpm workspace is setup.

## Tasks Setup

- Create `.moon/tasks/vite-frontend.yml`. (Skip if this already exists)
```yaml
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

## 🔧 Toolchain Setup

* Add to `.moon/toolchain.yml`: (skip if already exists)
```yaml
# Enable JavaScript
javascript:
  packageManager: 'pnpm'

# Enable Node.js and pnpm
node: {}
pnpm: {}
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

* Create a new Vite project under `apps`:
```bash
pnpm create vite

// or
CI=true pnpm create vite <appName> --template react-complier-ts
```
- Enter app name
- typescript + react compiler
- oxlint 

- Create Vite will typically add:
	- `index.html`, `src/`, `vite.config.ts`, `public/`
	- `package.json` with `scripts/dev` / `build` /  `lint` / `preview`
	- `tsconfig.json` + `tsconfig.app.json` + `tsconfig.node.json`
	- Its own `typescript`, `oxlint`, React Compiler (Babel + `@rolldown/plugin-babel`)
	- A nested `.gitignore` and README

* Create `moon.yml` in the project folder:
```yaml
language: 'typescript'
layer: 'application'  # or 'library' for packages
stacks: 'frontend'

project:
  title: '<title>'
  description: '<description>'
```
- language, layer, stacks will match `.moon/tasks/vite-frontend.yml`
- Tasks will be inherited from the yaml.

* Update `package.json`:
```json
{
  "name": "@<org>/<project>",
  "version": "0.0.0",
  "private": true
}
```
- Delete `scripts`
- Delete `typescript` from devDependencies. Already in root
- Delete `@types/node`. Root already has it.

- Refresh lockfile after edits (from repo root)

```bash
pnpm install
```

* Install Vitest:
```bash
// from repo root.  remove --filter @moon/<appName> if you are in the app directory
pnpm add -D vitest jsdom --filter @moon/<appName>
```

- Confirm there is **NO** `tsdown` or `tsx` on this package.

- Typescript updates
	- Delete `tsconfig.app.json` and `tsconfig.node.json`
	- `tsconfig.json`
	```json
	{
		"extends": "../../tsconfig.options.json",
		"compilerOptions": {
			"noEmit": true,
			"moduleReslution": "bundler",
			"jsx": "react-jsx",
			"lib": ["ES2022", "DOM", "DOM.Iterable"]
		},
		"include": ["src/**/*", "tests/**/*"],
		"references": []
	}
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


## 📝 Notes

* Do not create Vite projects at the workspace root — always inside `apps/` or `packages/`
* Tasks are inherited via moon preset tags — no need to define them manually
* `private: true` in `package.json` prevents accidental publishing to npm
* Use `@<org>/<project>` naming for all projects — e.g. `@huddle-up/web`
* Run `Moonrepo Toolchain - TypeScript Config` to set up shared tsconfig files
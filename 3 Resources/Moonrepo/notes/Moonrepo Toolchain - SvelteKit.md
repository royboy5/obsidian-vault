## 🌙 Moonrepo Toolchain: SvelteKit (Vite + Vitest)

[Moon SvelteKit Docs](https://moonrepo.dev/docs/guides/examples/sveltekit)

Not the JS default path (that is [[Moonrepo Toolchain - React Vite]] + [[Moonrepo Toolchain - Biome]]).

## 📁 Project Setup

* Create the project folder in the appropriate location:
```bash
# For deployable applications
mkdir apps/<project>

# For shared libraries / packages
mkdir packages/<project>

cd apps/<project>  # or packages/<project>
```

* Create a new SvelteKit project **inside** `apps/<id>` (not the workspace root). In a pnpm workspace prefer `pnpm create svelte@latest .` over `npm create`.
```bash
pnpm create svelte@latest .
```
* When prompted, select TypeScript and Vitest as needed. JS default formatter/linter is Biome — skip ESLint/Prettier unless this repo is not on Biome.

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
layer: 'application'
language: 'typescript'
stack: 'frontend'
```

## 🔧 Toolchain Setup

* Open `.moon/toolchains.yml`. For a JS workspace (`packageManager` under `javascript`, not `node`; versions in `.prototools`):

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

* Add tags to project `moon.yml` using official preset (recommended):
```yaml
layer: 'application'
language: 'typescript'
stack: 'frontend'
tags: ['sveltekit']
```

* Add `svelte.config.js` to project root:
```js
import adapter from '@sveltejs/adapter-auto';
import { vitePreprocess } from '@sveltejs/kit/vite';

/** @type {import('@sveltejs/kit').Config} */
const config = {
  preprocess: vitePreprocess(),
  kit: {
    adapter: adapter(),
  },
};

export default config;
```

## ESLint Integration

Skip this section if the repo is on Biome. If you are not on Biome:

* Add to project `moon.yml`:
```yaml
tasks:
  lint:
    args:
      - '--ext'
      - '.ts,.svelte'
```

* Add `.eslintrc.cjs` to project root:
```js
module.exports = {
  plugins: ['svelte3'],
  ignorePatterns: ['*.cjs'],
  settings: {
    'svelte3/typescript': () => require('typescript'),
  },
  overrides: [{ files: ['*.svelte'], processor: 'svelte3/svelte3' }],
};
```

## TypeScript Integration

* Add to project `moon.yml`:
```yaml
workspace:
  inheritedTasks:
    exclude: ['typecheck']

tasks:
  check:
    command: 'svelte-check --tsconfig ./tsconfig.json'
    deps:
      - 'typecheck-sync'
    inputs:
      - '@group(svelte)'
      - 'tsconfig.json'
```

* Add `tsconfig.json` to project root if not auto-generated:
```json
{
  "extends": "./.svelte-kit/tsconfig.json",
  "compilerOptions": {
    "allowJs": true,
    "checkJs": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "skipLibCheck": true,
    "sourceMap": true,
    "strict": true
  }
}
```

## 📝 Notes

* SvelteKit uses `svelte-check` instead of `tsc` for typechecking — the standard `typecheck` task is excluded and replaced with `check`
* Do not create SvelteKit projects at the workspace root — always inside `apps/` or `packages/`
* `private: true` in `package.json` prevents accidental publishing to npm
* Use `@<org>/<project>` naming for all projects — e.g. `@huddle-up/web`
* Run [[Moonrepo Toolchain - TypeScript Config]] to set up shared tsconfig files
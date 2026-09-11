## 🌙 Moonrepo Toolchain: Biome

[Biome](https://biomejs.dev/guides/getting-started/) · [Moon task inheritance](https://moonrepo.dev/docs/concepts/task-inheritance)

JS default linter and formatter in this vault. There is no moon Biome handbook; the task file is the same pattern as moon’s [ESLint example](https://moonrepo.dev/docs/guides/examples/eslint) (`inheritedBy` + `lint` / `format`). Use [[Moonrepo Toolchain - ESLint]] / [[Moonrepo Toolchain - Prettier]] only if the repo is **not** on Biome.

`tsc` typechecks. Biome lints and formats. Do not also install ESLint and Prettier on the same JS repo.

When done:

| Job | Tool | Moon task |
|-----|------|-----------|
| Lint (CI) | `biome check .` | `:lint` |
| Format (local) | `biome check --write .` | `:format` |

## Prerequisites

- Moon workspace exists. See [[Moonrepo Workspace Initialization]] if not.
- One root `biome.json`. No per-app `biome.json`.

## Tasks Setup

Create `.moon/tasks/biome.yml`. Skip if `lint` / `format` already exist.

```yaml
# Matches `language: typescript` on each project's moon.yml.
inheritedBy:
  languages: 'typescript'

fileGroups:
  sources:
    - 'src/**/*'
  tests:
    - 'tests/**/*'

tasks:
  lint:
    command:
      - 'biome'
      - 'check'
      - '.'
    inputs:
      - '@group(sources)'
      - '@group(tests)'
      - '*.config.*'
      - 'package.json'
      - 'tsconfig.json'
      - 'tsconfig.*.json'
      - '/biome.json'
  format:
    command:
      - 'biome'
      - 'check'
      - '--write'
      - '.'
    inputs:
      - '@group(sources)'
      - '@group(tests)'
      - '*.config.*'
      - 'package.json'
      - 'tsconfig.json'
      - 'tsconfig.*.json'
      - '/biome.json'
    options:
      runInCI: false
```

- Filename does not filter. `inheritedBy` does. Empty `inheritedBy` would give Biome to every project (including Rust/Go later).
- `*.config.*` is a cache input, not a Biome include. The command is `biome check .`, so `vite.config.ts` / `tsdown.config.ts` are still linted.
- `format` is write-mode and `runInCI: false`. CI runs `:lint` only (`biome check`, no `--write`).
- Do not copy this file onto the app `moon.yml`.

## Install

Workspace root (`-D` + `-w` + exact version). Do not use a dotted exact-flag typo.

```bash
pnpm add -D -wE @biomejs/biome
```

```bash
pnpx @biomejs/biome init
```

Then make `biome.json` match this workspace (tabs, skip `public/`). `dist/` is already skipped via `vcs.useIgnoreFile` and `.gitignore` — do not also list `!**/dist` unless you want that belt-and-suspenders.

```json
{
	"$schema": "https://biomejs.dev/schemas/2.5.11/schema.json",
	"vcs": {
		"enabled": true,
		"clientKind": "git",
		"useIgnoreFile": true
	},
	"files": {
		"ignoreUnknown": false,
		"includes": ["**", "!**/public"]
	},
	"formatter": {
		"enabled": true,
		"indentStyle": "tab"
	},
	"linter": {
		"enabled": true,
		"rules": {
			"preset": "recommended"
		}
	},
	"javascript": {
		"formatter": {
			"quoteStyle": "double"
		}
	},
	"assist": {
		"enabled": true,
		"actions": {
			"source": {
				"organizeImports": "on"
			}
		}
	}
}
```

- `indentStyle: tab` — this is the live moonrepo workspace, not a 2-space template.
- `!**/public` skips Create Vite favicons/sprites (`lint/a11y/noSvgWithoutTitle`). Those files are static assets. Do not add a per-app `biome.json` to silence that rule.

`:format` / `:lint` both run `biome check`, so **lint errors also fail format**. Format first, then lint:

```bash
moon run <id>:format
moon run <id>:lint
```

## 📝 Notes

- Biome is not a moon toolchain. Nothing in `.moon/toolchains.yml` creates these tasks.
- Run from the **repo root** (`moon run dashboard:lint`). Task cwd is the project.
- See [[Moonrepo Toolchain - TypeScript Config]] for `:typecheck`.
- See [[Moonrepo Toolchain - React Vite]] / [[Moonrepo Toolchain - Node TypeScript]] for app wiring.
- SPA `deps`: [[Moonrepo Task Dependencies]].

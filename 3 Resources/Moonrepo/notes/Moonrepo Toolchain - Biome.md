## 🌙 Moonrepo Toolchain: Biome

[Moon ESLint Docs](https://moonrepo.dev/docs/guides/examples/eslint)

## 🔧 Setup

* Install at the workspace root:
```bash
pnpm add -D -w -.E @biomejs/biome
```

- Run init
```bash
pnpx @biomejs/biome init
```

- `biome.json` (root)
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
		"includes": ["**", "!**/dist", "!**/build", "!**/coverage"]
	},
	"formatter": {
		"enabled": true,
		"indentStyle": "space",
		"lineWidth": 100
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

* Create `.moon/tasks/biome.yml`:
```yaml
inheritedBy:
  languages: typescript

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
      - 'package.json'
      - 'tsconfig.json'
      - 'tsconfig.*.json'
      - '/biome.json'
    options:
      runInCI: false
```

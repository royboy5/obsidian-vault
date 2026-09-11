## 🌙 Moonrepo Toolchain: Prettier

[Moon Prettier Docs](https://moonrepo.dev/docs/guides/examples/prettier)

JS default in this vault is [[Moonrepo Toolchain - Biome]]; use this only if the repo is not on Biome.

## 🔧 Setup

* Install at the workspace root:
```bash
pnpm add -D -w prettier
```

* Create `.moon/tasks/prettier.yml`:
```yaml
inheritedBy:
  languages: 'typescript'

tasks:
  format:
    command:
      - 'prettier'
      # Use the same config for the entire repo
      - '--config'
      - '@in(4)'
      # Use the same ignore patterns as well
      - '--ignore-path'
      - '@in(3)'
      # Fail for unformatted code
      - '--check'
      # Run in current dir
      - '.'
    inputs:
      # Source and test files
      - 'src/**/*'
      - 'tests/**/*'
      # Config and other files
      - '**/*.{md,mdx,yml,yaml,json}'
      # Root configs, any format
      - '/.prettierignore'
      - '/.prettierrc.*'
```

* Create root `.prettierrc.js`:
```js
module.exports = {
  arrowParens: 'always',
  semi: true,
  singleQuote: true,
  tabWidth: 2,
  trailingComma: 'all',
  useTabs: true,
};
```

* Create root `.prettierignore`:
```bash
node_modules/
*.min.js
*.map
*.snap
```

## 📝 Notes

* Root-level config is required — Prettier should use the same standards across the whole repo
* Only 1 `.prettierignore` file is supported per repo — always define it at the root
* Empty `inheritedBy` would give Prettier to every project. Filter with `inheritedBy.languages` (filename does not filter)
* Moon recommends against project-level Prettier configs — use root overrides if you need escape hatches during migrations
* `--check` is used in CI; configure your editor to run Prettier on save for local formatting
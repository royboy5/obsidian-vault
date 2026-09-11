## 🌙 Moonrepo Toolchain: ESLint

[Moon ESLint Docs](https://moonrepo.dev/docs/guides/examples/eslint)

JS default in this vault is [[Moonrepo Toolchain - Biome]]; use this only if the repo is not on Biome.

## 🔧 Setup

* Install at the workspace root:
```bash
pnpm add -D -w eslint eslint-config-moon
```

* Create `.moon/tasks/eslint.yml`. `lint` is check-only (CI). Do **not** put `--fix` on `lint`.
```yaml
inheritedBy:
  languages: 'typescript'

tasks:
  lint:
    command:
      - 'eslint'
      - '--ext'
      - '.js,.jsx,.ts,.tsx'
      - '--report-unused-disable-directives'
      - '--no-error-on-unmatched-pattern'
      - '--exit-on-fatal-error'
      - '--ignore-path'
      - '@in(4)'
      - '.'
    inputs:
      - 'src/**/*'
      - 'tests/**/*'
      - '*.config.*'
      - '**/.eslintrc.*'
      - '/.eslintignore'
      - '/.eslintrc.*'
  format:
    command:
      - 'eslint'
      - '--ext'
      - '.js,.jsx,.ts,.tsx'
      - '--fix'
      - '--no-error-on-unmatched-pattern'
      - '.'
    options:
      runInCI: false
```

## TypeScript Integration

- Add `@typescript-eslint` package to root.
```bash
pnpm add -D -w eslint @eslint/js typescript typescript-eslint
```

* Create root `tsconfig.eslint.json`:
```json
{
  "extends": "./tsconfig.options.json",
  "compilerOptions": {
    "emitDeclarationOnly": false,
    "noEmit": true
  },
  "include": ["apps/**/*", "packages/**/*"]
}
```

* Add TypeScript inputs to `.moon/tasks/eslint.yml`:
```yaml
tasks:
  lint:
    inputs:
      - 'types/**/*'
      - 'tsconfig.json'
      - '/tsconfig.eslint.json'
      - '/tsconfig.options.json'
```

## Per-project Overrides (optional)

* Add to `<project>/moon.yml` to extend the global lint task:
```yaml
tasks:
  lint:
    args:
      - '--cache'
```

* Add `<project>/.eslintrc.js` for project-specific rules:
```js
module.exports = {
  ignorePatterns: ['build', 'lib'],
  rules: {
    'no-console': 'off',
  },
};
```

> ***The extends setting should not extend the root-level config, as ESLint will automatically merge configs while traversing upwards!***

## Root-level Config

* Create root `.eslintrc.js`:
```js
module.exports = {
  root: true,
  extends: ['moon'],
  rules: {
    'no-console': 'error',
  },
  parser: '@typescript-eslint/parser',
  parserOptions: {
    project: 'tsconfig.eslint.json',
    tsconfigRootDir: __dirname,
  },
};
```

* Create root `.eslintignore`:
```bash
node_modules/
*.min.js
*.map
*.snap
```

## 📝 Notes

* `root: true` in `.eslintrc.js` is required — it tells ESLint to stop traversing upwards
* Only 1 `.eslintignore` file is supported per repo — always define it at the root
* Empty `inheritedBy` would give ESLint to every project. Filter with `inheritedBy.languages` (filename does not filter)
* `--fix` belongs on `format` with `runInCI: false`, never on the CI `lint` task
* Do not use `extends` in project-level ESLint configs — ESLint merges configs automatically while traversing upwards
## 🌙 Moonrepo Toolchain: ESLint

[Moon ESLint Docs](https://moonrepo.dev/docs/guides/examples/eslint)

## 🔧 Setup

* Install at the workspace root:
```bash
pnpm add -D -w eslint eslint-config-moon
```

* Create `.moon/tasks/eslint.yml`:
```yaml
tasks:
  lint:
    command:
      - 'eslint'
      # Support other extensions
      - '--ext'
      - '.js,.jsx,.ts,.tsx'
      # Always fix and run extra checks
      - '--fix'
      - '--report-unused-disable-directives'
      # Dont fail if a project has nothing to lint
      - '--no-error-on-unmatched-pattern'
      # Do fail if we encounter a fatal error
      - '--exit-on-fatal-error'
      # Only 1 ignore file is supported, so use the root
      - '--ignore-path'
      - '@in(4)'
      # Run in current dir
      - '.'
    inputs:
      # Source and test files
      - 'src/**/*'
      - 'tests/**/*'
      # Other config files
      - '*.config.*'
      # Project configs, any format, any depth
      - '**/.eslintrc.*'
      # Root configs, any format
      - '/.eslintignore'
      - '/.eslintrc.*'
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
* Tasks in `.moon/tasks/` are inherited automatically by all projects — no need to add `lint` to each `moon.yml`
* Do not use `extends` in project-level ESLint configs — ESLint merges configs automatically while traversing upwards
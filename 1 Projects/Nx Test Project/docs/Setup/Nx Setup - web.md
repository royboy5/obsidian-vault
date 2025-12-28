# 🚀 Nx Setup: web

**Link:** [[Nx Monorepo MOC]]

### ⚛️ React Setup: web

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm add -D @nx/react`

#### 2. 🏗️ Generate Application
- [ ] **Run generator:**
```bash
pnpm nx g @nx/react:app packages/client/web
```
or,
```bash
pnpm nx g @nx/react:app web \
  --directory=apps/client/web \
  --bundler=vite \
  --style=css \
  --e2eTestRunner=cypress \
  --unitTestRunner=vitest \
  --linter=eslint
  ```

#### 3. ✅ Verify Targets
- [ ] Check apps/web/project.json:
	- [ ] serve (Dev)
	- [ ] build (Prod)
	- [ ] lint

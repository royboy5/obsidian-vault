### 🟢 Node Setup: {{APP_NAME}}

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm add -D @nx/node`

#### 2. 🏗️ Generate Application
- [ ] **Run generator:**
```bash
pnpm nx g @nx/node:application apps/api/{{APP_NAME}}
```

#### 3. ✅ Verify Targets
- [ ] Check apps/api/{{APP_NAME}}/project.json:
	- [ ] serve (Dev)
	- [ ] build (Prod)
	- [ ] lint
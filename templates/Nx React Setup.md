### ⚛️ React Setup: {{APP_NAME}}

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm nx add @nx/react`

#### 2. 🏗️ Generate Application
- [ ] **Run generator:**
```bash
pnpm nx g @nx/react:{{app | lib}} {{APP PATH}}/{{APP_NAME}}
```

#### 3. ✅ Verify Targets
- [ ] Check apps/client/{{APP_NAME}}/project.json:
	- [ ] serve (Dev)
	- [ ] build (Prod)
	- [ ] lint
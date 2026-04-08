---
status: todo
stack: "[[Node API]]"
domain: "Shared"
port: "4200"
related_moc: "[[Nx Monorepo MOC]]"
created: 2026-04-08
tags:
  - nx-setup
---

# 🚀 Nx Setup: api5

### 🟢 Node Setup: api5

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm nx add @nx/node`

#### 2. 🏗️ Generate API
- [ ] **Run generator:**
```bash
pnpm nx g @nx/node:app api5 \
  --directory=apps/api5 \
  --port=4200 \
  --framework=none --linter=eslint --unitTestRunner=none --e2eTestRunner=none --useProjectJson=true
```

#### 3. ✅ Verify Targets
- [ ] Check apps/api5/project.json
	- [ ] serve (Dev)
	- [ ] build (Prod)
	- [ ] lint

#### 4. TSX (OPTIONAL)
- [ ] Install package
```bash
pnpm add -wD tsx
```
- [ ] Configure `project.json`
```bash
"serve": {
  "executor": "nx:run-commands",
  "options": {
    "command": "tsx watch src/main.ts",
    "cwd": "apps", // i.e "apps/api", 
    "color": true
  }
}
```


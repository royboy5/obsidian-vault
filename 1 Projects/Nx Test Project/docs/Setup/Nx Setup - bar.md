---
status: todo
stack: "[[Node API]]"
domain: "Shared"
port: "5000"
related_moc: "[[Nx Monorepo MOC]]"
created: 2026-01-02
tags:
  - nx-setup
---

# 🚀 Nx Setup: bar

### 🟢 Node Setup: bar

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm nx add @nx/node`

#### 2. 🏗️ Generate Application
- [ ] **Run generator:**
```bash
pnpm nx g @nx/node:{{app | lib}} {{PATH}}/bar
```

#### 3. ✅ Verify Targets
- [ ] Check apps/api/bar/project.json:
	- [ ] serve (Dev)
	- [ ] build (Prod)
	- [ ] lint

#### 4. TSX
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
    "cwd": "{{DIR OF APP / LIB}}", // i.e "apps/api", 
    "color": true
  }
}
```


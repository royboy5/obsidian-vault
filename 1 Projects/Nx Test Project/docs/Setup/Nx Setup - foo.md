---
status: todo
stack: "[[Node Shared Lib]]"
domain: "Shared"
port: ""
related_moc: "[[Nx Monorepo MOC]]"
created: 2026-01-02
tags:
  - nx-setup
---

# 🚀 Nx Setup: foo

### 🟢 Node Setup: foo

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm nx add @nx/node`

#### 2. 🏗️ Generate Application
- [ ] **Run generator:**
```bash
pnpm nx g @nx/node:{{app | lib}} {{PATH}}/foo
```

#### 3. ✅ Verify Targets
- [ ] Check apps/api/foo/project.json:
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


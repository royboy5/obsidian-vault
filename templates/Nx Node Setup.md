### 🟢 Node Setup: {{APP_NAME}}

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm nx add @nx/node`

#### 2. 🏗️ Generate Application
- [ ] **Run generator:**
```bash
pnpm nx g @nx/node:{{app | lib}} {{PATH}}/{{APP_NAME}}
```

#### 3. ✅ Verify Targets
- [ ] Check apps/api/{{APP_NAME}}/project.json:
	- [ ] serve (Dev)
	- [ ] build (Prod)
	- [ ] lint

#### 4. TSX
- [ ] Install package
```bash
pnpm add -D tsx
```
- [ ] Configure `project.json`
```bash
"serve": {
  "executor": "nx:run-commands",
  "options": {
    "command": "npx tsx watch src/main.ts",
    "cwd": "{{DIR OF APP / LIB}}", // i.e "apps/api", 
    "color": true
  }
}
```


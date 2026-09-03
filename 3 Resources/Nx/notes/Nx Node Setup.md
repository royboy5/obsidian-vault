### 🟢 Node Setup: {{APP_NAME}}

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm nx add @nx/node`

#### 2. 🏗️ Generate {{TYPE_LABEL}}
[Nx Docs Reference - Node Generators](https://nx.dev/docs/technologies/node/generators)
- [ ] **Run generator:**
```bash
pnpm nx g @nx/node:{{TYPE}} {{APP_NAME}} \
  --directory={{DIRECTORY}}/{{APP_NAME}} \
  {{PORT}} \
  {{FLAGS}}
```

#### 3. ✅ Verify Targets
- [ ] Check {{DIRECTORY}}/{{APP_NAME}}/project.json
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
    "cwd": "{{DIRECTORY}}", // i.e "apps/api", 
    "color": true
  }
}
```


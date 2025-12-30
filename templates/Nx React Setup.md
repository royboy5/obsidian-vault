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

#### 4. Make sure the backend is running
- When developing locally with a backend, you may want the backend api to automatically start when you run `pnpm nx serve {{APP_NAME}}`. To do this, edit the `package.json` with,
```json
  "nx": {
    "targets": {
      "serve": {
        "dependsOn": [
          "@newrepo/api:serve"
        ]
      }
    }
  }
```
or,
```json
 "nx": {
    "targets": {
      "serve": {
        "dependsOn": [
          {
            "target": "serve",
            "projects": ["api"]
          }
        ]
      }
    }
  }
```

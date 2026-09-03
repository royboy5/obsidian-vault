### ⚛️ React {{TYPE_LABEL}}: {{APP_NAME}}

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm nx add @nx/react`

#### 2. 🏗️ Generate {{TYPE_LABEL}}
- [ ] **Run generator:**
```bash
pnpm nx g @nx/react:{{TYPE}} {{APP_NAME}} \
  --directory={{DIRECTORY}}/{{APP_NAME}} \
  {{PORT}} \
  {{FLAGS}}
```

#### 3. ✅ Verify Targets
- [ ] Check {{DIRECTORY}}/{{APP_NAME}}/project.json
	- [ ] serve (Dev)
	- [ ] build (Prod)
	- [ ] lint

#### 4. Make sure the backend is running (OPTIONAL)
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

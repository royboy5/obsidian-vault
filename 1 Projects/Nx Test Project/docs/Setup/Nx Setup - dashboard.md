---
status: todo
stack: "[[React App]]"
domain: "Shared"
port: "3000"
related_moc: "[[Nx Monorepo MOC]]"
created: 2026-04-08
tags:
  - nx-setup
---

# 🚀 Nx Setup: dashboard

### ⚛️ React Application: dashboard

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
  `pnpm nx add @nx/react`

#### 2. 🏗️ Generate Application
- [ ] **Run generator:**
```bash
pnpm nx g @nx/react:app dashboard \
  --directory=apps/dashboard \
  --port=3000 \
  --bundler=vite --style=css --useProjectJson=true
```

#### 3. ✅ Verify Targets
- [ ] Check apps/dashboard/project.json
	- [ ] serve (Dev)
	- [ ] build (Prod)
	- [ ] lint

#### 4. Make sure the backend is running (OPTIONAL)
- When developing locally with a backend, you may want the backend api to automatically start when you run `pnpm nx serve dashboard`. To do this, edit the `project.json` with,
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

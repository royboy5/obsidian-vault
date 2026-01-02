---
status: todo
stack: "[[UI Kit]]"
domain: "Shared"
port: "4040"
related_moc: "[[Nx Monorepo MOC]]"
created: 2026-01-02
tags:
  - nx-setup
---

# 🚀 Nx Setup: sharedui

### ⚛️ React UI Library: sharedui

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
```bash
pnpm add -D @nx/react @nx/storybook
```

#### 2. 🏗️ Generate {{TYPE_LA[[Nx React Setup]]BEL}}
- [ ] **Run React generator:**
```bash
pnpm nx g @nx/react:lib sharedui \
  --directory=libs/shared/sharedui \
  --port=4040 \
  {{FLAGS}}
```
- [ ] **Run Storybook generator:**
```bash
pnpm nx g @nx/react:storybook-configuration sharedui \
  --interactionTests=true \
  --generateStories=true
```
#### 3. ⚙️ Configure Port --port=4040
Storybook defaults to port 4400. To avoid conflicts, let's set a specific one.
- [ ] Open libs/shared/sharedui/project.json
- [ ] Find the storybook target -> options
- [ ] Add/Update: "port": --port=4040
#### 4. ✅ Verify Targets
- [ ] Check libs/shared/sharedui/project.json
	- [ ] serve (Dev)[[Nx React Setup]]
	- [ ] build (Prod)
	- [ ] lint
#### 5. Make sure the backend is running (OPTIONAL)
- When developing locally with a backend, you may want the backend api to automatically start when you run `pnpm nx serve sharedui`. To do this, edit the `package.json` with,
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

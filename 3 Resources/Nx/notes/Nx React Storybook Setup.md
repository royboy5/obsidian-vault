### ⚛️ React {{TYPE_LABEL}}: {{APP_NAME}}

#### 1. 🔌 Install Plugin
- [ ] **Run in terminal:**
```bash
pnpm add -D @nx/react @nx/storybook
```

#### 2. 🏗️ Generate {{TYPE_LA[[Nx React Setup]]BEL}}
- [ ] **Run React generator:**
```bash
pnpm nx g @nx/react:{{TYPE}} {{APP_NAME}} \
  --directory={{DIRECTORY}}/{{APP_NAME}} \
  {{PORT}} \
  {{FLAGS}}
```
- [ ] **Run Storybook generator:**
```bash
pnpm nx g @nx/react:storybook-configuration {{APP_NAME}} \
  --interactionTests=true \
  --generateStories=true
```
#### 3. ⚙️ Configure Port {{PORT}}
Storybook defaults to port 4400. To avoid conflicts, let's set a specific one.
- [ ] Open {{DIRECTORY}}/{{APP_NAME}}/project.json
- [ ] Find the storybook target -> options
- [ ] Add/Update: "port": {{PORT}}
#### 4. ✅ Verify Targets
- [ ] Check {{DIRECTORY}}/{{APP_NAME}}/project.json
	- [ ] serve (Dev)[[Nx React Setup]]
	- [ ] build (Prod)
	- [ ] lint
#### 5. Make sure the backend is running (OPTIONAL)
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

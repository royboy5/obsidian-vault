# 🦋 Setup: Nx Monorepo

#### 📋 Prerequisites
- [ ] **Node.js:** Verify installed (`node -v`)
- [ ] **Nx CLI:** Verify global install (`nx --version`)
    - *If missing:* `pnpm install -g nx@latest`

#### 🏗️ Workspace Creation
#### Manual

- [ ] Create and enter directory 
```bash
mkdir my-new-workspace && cd my-new-workspace
```

- [ ] Initialize git
```bash
git init
```

- [ ] Create `.gitignore`
```bash
echo "node_modules\n.DS_Store\ndist\ntmp\n.nx/cache" > .gitignore
```

- [ ] Initialize package manager
```bash
pnpm init
```

* [ ] Update Root Package Name
`pnpm init` defaults the name to the folder name (e.g., `"my-monorepo"`). Change this to your scoped organization name immediately to establish ownership.
* **Action:** Open `package.json`
* **Change:** `"name": "my-monorepo"` → `"name": "@mycompany/source"` (or `@mycompany/root`)
* *Tip:* Also ensure `"private": true` is set to prevent accidental publishing of the entire repo.

- [ ] create `pnpm-workspace.yaml`
```yaml
packages:
	- 'apps/*'
	- 'libs/*'
 ```

- [ ] `nx init`
- [ ] Continue with specific `Nx bridge setup`

#### Nx Guided
- [ ] **Init Workspace:** `pnpm dlx create-nx-workspace {{scope}} --workspaces`
    - [ ] **Name:** (e.g., `my-org` or `my-project`)
    - [ ] **Stack:** Choose `Integrated` (Strict plugins) or `Package-based` (Flexible/Turborepo style)
    - [ ] **CI:** Select `GitHub Actions` or `Skip` for now
- [ ] **Navigate:** `cd <workspace-name>`
- [ ] **Git:** Verify `git init` ran automatically. If not, run it.

**NOTES:***
- Some of the generated code varies with the framework selected.  For example, some uses @org/source and some uses @app-name
- 

#### ⚙️ Tooling & Config
- [ ] **VS Code:** Install the **Nx Console** extension.
- [ ] **Prettier:** Ensure `.prettierrc` exists in the root.
- [ ] **Nx Cloud:** Run `npx nx connect` (Optional: Enables remote caching).

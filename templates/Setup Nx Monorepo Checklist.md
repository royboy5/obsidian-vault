# 🦋 Setup: Nx Monorepo

#### 📋 Prerequisites
- [ ] **Node.js:** Verify installed (`node -v`)
- [ ] **Nx CLI:** Verify global install (`nx --version`)
    - *If missing:* `pnpm install -g nx@latest`

#### 🏗️ Workspace Creation
- [ ] **Init Workspace:** `npx create-nx-workspace@latest`
    - [ ] **Name:** (e.g., `my-org` or `my-project`)
    - [ ] **Stack:** Choose `Integrated` (Strict plugins) or `Package-based` (Flexible/Turborepo style)
    - [ ] **CI:** Select `GitHub Actions` or `Skip` for now
- [ ] **Navigate:** `cd <workspace-name>`
- [ ] **Git:** Verify `git init` ran automatically. If not, run it.

#### ⚙️ Tooling & Config
- [ ] **VS Code:** Install the **Nx Console** extension.
- [ ] **Prettier:** Ensure `.prettierrc` exists in the root.
- [ ] **Nx Cloud:** Run `npx nx connect` (Optional: Enables remote caching).

#### 📦 First App (React/Node)
- [ ] **Install Plugin:** `npm install -D @nx/react` (or `@nx/node`)
- [ ] **Generate App:** `npx nx g @nx/react:app my-app`
- [ ] **Serve:** `npx nx serve my-app`
- [ ] **Visualize:** Run `npx nx graph` to see the dependency diagram.

#### 🐍 Python Integration (Optional)
- [ ] **Install Bridge:** `npm install -D @nxlv/python` (or similar plugin)
- [ ] **Update Config:** Check `nx.json` for python-specific settings.
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

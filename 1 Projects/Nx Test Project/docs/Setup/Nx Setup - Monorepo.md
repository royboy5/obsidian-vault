# 🦋 Setup: Nx Monorepo

#### 📋 Prerequisites
- [ ] **Node.js:** Verify installed (`node -v`)
- [ ] **Nx CLI:** Verify global install (`nx --version`)
    - *If missing:* `pnpm add -g nx@latest`

#### 🏗️ Workspace Creation

#### Nx Guided

- [x] **Init Workspace:** `pnpm dlx create-nx-workspace {{scope}} --workspaces`
    - [x] **Name:** (e.g., `my-org` or `my-project`)
    - [x] ***starter***: `Custom`
    - [x] **Stack:** Choose `None` 
- [x] **Navigate:** `cd <workspace-name>`
- [ ] **Git:** Verify `git init` ran automatically. If not, run it.
- [ ] Continue with specific `Nx bridge setup`

**NOTES:***
- Some of the generated code varies with the framework selected.  For example, some uses @org/source and some uses @app-name
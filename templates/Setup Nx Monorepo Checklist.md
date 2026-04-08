# 🦋 Setup: Nx Monorepo

#### 📋 Prerequisites
- [ ] **Node.js:** Verify installed (`node -v`)
- [ ] **Nx CLI:** Verify global install (`nx --version`)
    - *If missing:* `pnpm add -g nx@latest`

#### 🏗️ Workspace Creation

#### Nx Guided

- [ ] **Init Workspace:** `pnpm dlx create-nx-workspace {{name}} --analytics=false --appName={myApp} --docker=false e2eTestRunner-none --formatter=prettier --framework=none --name=myOrg --nxCloud=skip --packageManager=pnpm --preset=apps --unitTestRunner=vitest --useProjectJson=true --workspaceType=integrated --workspace`
    - [ ] **Name:** (e.g., `my-org` or `my-project`)
    - [ ] ***myApp***: `Node App Name`
- [ ] **Navigate:** `cd <workspace-name>`
- [ ] **Git:** Verify `git init` ran automatically. If not, run it.
- [ ] Continue with specific `Nx bridge setup`

**NOTES:***
- Some of the generated code varies with the framework selected.  For example, some uses @org/source and some uses @app-name

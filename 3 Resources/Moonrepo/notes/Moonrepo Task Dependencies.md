## 🌙 Moonrepo Task Dependencies

[Moon tasks](https://moonrepo.dev/docs/concepts/task) · [Moon targets](https://moonrepo.dev/docs/concepts/target) · [Project `dependsOn` / task `deps`](https://moonrepo.dev/docs/config/project)

Moon has **two** dependency lists. Mixing them up is the usual failure.

| Setting | Lives on | Graph | Job |
|---------|----------|-------|-----|
| `dependsOn` | project `moon.yml` | **project** graph | This app uses that project (library, API, etc.) |
| `tasks.<name>.deps` | inherited `.moon/tasks/*.yml` **or** project `moon.yml` | **task** graph | Run that **target** before this task |

`dependsOn: ['backend']` does **not** start the API. `tasks.dev.deps: ['backend:dev']` does.

A **target** is `project:task` (`dashboard:dev`, `backend:build`). Same as `moon run dashboard:dev`.

## Where to put task `deps`

Put the dep on the **narrowest** file that should always get it.

| You want | Put `deps` here | Why |
|----------|-----------------|-----|
| Every frontend `dev` starts the API | `.moon/tasks/vite-frontend.yml` → `tasks.dev` | All `stack: frontend` apps inherit it |
| Only dashboard starts the API | `apps/dashboard/moon.yml` → `tasks.dev` | `web` stays Vite-only |
| Every backend `start` after bundle | `.moon/tasks/node-backend.yml` → `tasks.start` | Already `deps: ['build']` |
| Preview after build | `.moon/tasks/vite-frontend.yml` → `tasks.preview` | Already `deps: ['build']` |
| One-off, no YAML | CLI | `moon run dashboard:dev backend:dev` |

Inherited tasks **merge**. The app `moon.yml` can add `deps` without repeating `command: 'vite'`. Do not copy the whole `dev` task into the app unless you are replacing it.

Do **not** put cross-app `dev` deps in `.moon/tasks/vite-frontend.yml` unless every SPA should boot the API.

## Project `dependsOn`

On the **consumer** app (`apps/dashboard/moon.yml`), not on the API:

```yaml
dependsOn:
  - 'backend'
```

Use this when:

- The SPA (or a library) is a real dependency of that project in the graph
- You want `^:build` / `^:dev` to expand to those projects
- moon / TypeScript project-reference sync should know the edge

`dependsOn` alone does **not** run `:dev`. Pair it with task `deps` (explicit `backend:dev` or `^:dev`).

## Target shortcuts

| Dep | Means |
|-----|--------|
| `backend:dev` | That project, that task |
| `build` or `~:build` | Same project |
| `^:dev` | `:dev` on every project in this app’s `dependsOn` |

Prefer an explicit `backend:dev` when only one API should start. Use `^:dev` when the SPA lists exactly the watchers it needs in `dependsOn`.

## Persistent tasks (servers and watchers)

Tasks that **never exit** (`vite`, `tsx watch`, `vite preview`) must be `options.persistent: true`.

Moon collects persistent tasks and runs them **last, in parallel**, after non-persistent deps finish. That is how API + Vite start together.

If the API watcher is **not** persistent:

1. `dashboard:dev` waits on `backend:dev`
2. `tsx watch` never finishes
3. Vite never starts

Both sides need it:

```yaml
# .moon/tasks/node-backend.yml
tasks:
  dev:
    command: 'tsx watch src/index.ts'
    options:
      persistent: true
```

```yaml
# .moon/tasks/vite-frontend.yml (already)
tasks:
  dev:
    command: 'vite'
    options:
      persistent: true
```

Local-only watchers should also stay out of CI (`runInCI: false` if moon would otherwise run them). `dev` is usually skipped in CI because it is persistent.

Use `backend:dev` for local HMR. Do **not** depend on `backend:start` for that — `start` is `node dist/index.js` after `build`, not the watcher.

## Recipe: start the API when you start the dashboard

**1.** Persistent API watcher (once, inherited):

`.moon/tasks/node-backend.yml` — `dev` has `options.persistent: true` (snippet above).

**2.** Only this SPA:

`apps/dashboard/moon.yml`:

```yaml
layer: 'application'
language: 'typescript'
stack: 'frontend'

dependsOn:
  - 'backend'

project:
  title: 'dashboard'
  description: 'React Vite frontend application.'

tasks:
  dev:
    deps:
      - 'backend:dev'
```

No `command:` here. Inherited task is still `vite`.

**3.** Run from the workspace root:

```bash
moon run dashboard:dev
```

Moon runs `backend:dev` and `dashboard:dev` together.

Check the graph:

```bash
moon project dashboard
moon task dashboard:dev
```

`dev` should list `backend:dev` under deps. `build` / `lint` / `test` should **not**.

## Same idea, other apps

- **Library then app:** `dependsOn: ['ui']` on the app; `tasks.build.deps: ['^:build']` if you need their artifacts first. Apps that `noEmit` usually do not wait on a library `dev`.
- **Two SPAs, one API:** put `backend:dev` on each SPA that needs it, or put `dependsOn: ['backend']` + `^:dev` on those SPAs. Do not add it to `vite-frontend.yml` if `web` should stay standalone.
- **Preview / production-like:** `preview` already depends on same-project `build`. Do not add `backend:dev` there unless you really want the watcher next to `vite preview`.

## Task `deps` is not HTTP

Starting `backend:dev` does not point the browser at the API. You still need a Vite `server.proxy`, a `VITE_*` base URL, or CORS on the API. That lives in `vite.config.ts` / the server, not in `moon.yml`.

## What not to do

- Do not put `backend:dev` on `.moon/tasks/vite-frontend.yml` unless every frontend should start the API.
- Do not put `deps` on `lint` / `typecheck` / `test` just to boot servers. Those are CI gates.
- Do not use `inheritedTasks.exclude` to dodge a dep you put on the inherited file — move the dep to the app `moon.yml` instead.
- Do not add a `tasks:` block that redefines `command` unless you intend to replace Vite/tsdown.
- Do not expect `dependsOn` to start processes.

## Notes

- Project ids are folder names (`apps/backend` → `backend`), same as `moon run`.
- [[Moonrepo Toolchain - React Vite]] — SPA `dev` / `build` / `preview`
- [[Moonrepo Toolchain - Node TypeScript]] — API `dev` / `build` / `start`
- [[Moonrepo Task Inheritance Mechanisms]] — why inherited files vs app `moon.yml`

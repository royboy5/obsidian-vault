---
created: 2025-12-30 01:52
updated: 2025-12-30 01:52
tags:
  - notes
  - Nx
---

## Incorrect node version

## Core Idea
If you see this error,
```bash
pnpm nx serve web

> nx run @newrepo/web:serve

> vite

You are using Node.js 20.17.0. Vite requires Node.js version 20.19+ or 22.12+. Please upgrade your Node.js version.

```

And, when you run `node --version` and you get a different version, it may be `volta` not matching up `pnpm` node version.  

Run `volta install pnpm` to update `pnpm`
## Connections
*How does this idea relate to what I already know?*
- [volta](https://volta.sh/)

## Open Questions
*What does this make me wonder about? What am I still curious about?*
- 
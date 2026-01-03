---
created: 2026-01-01 17:14
updated: 2026-01-01 17:14
tags:
  - notes
---

## Module Boundaries

## Core Idea
*This is the single concept, explained in my own words.  This forces me to process the information (encode it) rather than just copy-pasting.*

[Nx - Enforce Module Boundaries](https://nx.dev/docs/features/enforce-module-boundaries#tags)

When breaking up code into different modules (aka libraries), we need to make sure we set some architectural boundaries to ensure projects can only have certain dependencies.  For example, a React component should not be able to import an API's database library.  This will break crash the browser.  

## Connections
*How does this idea relate to what I already know?*

How to enforce?
- ESLint Integration
	- JS / TS projects can enforce boundaries using `@nx/enforce-module-boundaries` ESLint rule.  This checks the `package.json` dependencies during linting.
- [Language-Agnostic Conformace](https://nx.dev/docs/enterprise/conformance)
	- Check dependencies in the Nx Graph during `nx conformance:check`.  Requires enterprise plan.
- Both use [[Nx Tags]]

## Open Questions
*What does this make me wonder about? What am I still curious about?*
- Q - How do I set boundaries on projects of different languages?
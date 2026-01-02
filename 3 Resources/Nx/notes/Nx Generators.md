---
created: 2025-12-30 02:21
updated: 2025-12-30 02:21
tags:
  - notes
---

## Nx Generators

## Core Idea
*This is the single concept, explained in my own words.  This forces me to process the information (encode it) rather than just copy-pasting.*

Nx Generators are the "Code Writers".  It creates / modifies files for you.  For example, creating new apps, setting up libraries, or configuring tools.  Can be though of as "Smart Blueprints".  

For example,
```bash
nx g @nx/react:app apps/my-react-app
```

Will create and scaffold a react app in your `apps/my-react-app` directory.  It will include linting, formatting, tsconfig, bundlers, etc.  
## Connections
*How does this idea relate to what I already know?*
- It's an example of ...
	- It's like the express generator `npx express-generator`
	- Also, Vite `pnpm create vite@latest`
	- 
- It's the opposite of ...
- It's used in ...

## Open Questions
*What does this make me wonder about? What am I still curious about?*
- Q - Can / How to create your own generators?
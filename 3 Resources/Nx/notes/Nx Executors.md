---
created: 2025-12-30 22:01
updated: 2025-12-30 22:01
tags:
  - notes
---

## Nx Executors

## Core Idea
*This is the single concept, explained in my own words.  This forces me to process the information (encode it) rather than just copy-pasting.*

[Nx Executors](https://nx.dev/docs/concepts/executors-and-configurations)
Executors are the "Task Runners".  Meaning, it is the tool that performs actions on your code.  Can think of them as wrappers around your commands like serve, build, test, etc.

In order to use an executor, you need to install the plugin that contains the executor and then configure the executor in the project's `project.json`

**Commands**
- `nx run`
- `nx build`
- `nx serve`
- `nx test`

**Example**
```bash
nx [commance] [project]
nx serve api
```

The executor is defined inside your `project.json` under the `targets` object.  Or in your `package.json` under the `nx: { targets }` object
## Connections
*How does this idea relate to what I already know?*
- It's an example of ...
	- Similar to `package.json` scripts, but with a lot more functionality and control.
- It's the opposite of ...
- It's used in ...

## Open Questions
*What does this make me wonder about? What am I still curious about?*
- Q - How does this apply to ...?
- Q - What is the difference between this and ...?
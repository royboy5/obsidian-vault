---
created: 2026-01-02 14:15
updated: 2026-01-02 14:15
tags:
  - notes
---

## Nx Tags

## Core Idea
*This is the single concept, explained in my own words.  This forces me to process the information (encode it) rather than just copy-pasting.*

[Nx Tags](https://nx.dev/docs/features/enforce-module-boundaries#tags)

Tags are metadata labels you put on your projects.  They serve 2 main purposes, 
- Filtering
- Enforcing boundaries

Tags are defined in your projects configuration file (`package.json` or `project.json`)

### Example 
Config with 3 tags: `scope:client`, `scope:admin`, `scope:shared`.

client/package.json
```json
{  
  // ... more project configuration here  
  "nx": {
    "tags": ["scope:client"],
  },
}
```

admin/package.json

```json
{  
  // ... more project configuration here
  "nx": {
    "tags": ["scope:admin"],
  },
}
```

utils/package.json

```json
{  
  // ... more project configuration here
  "nx": {
    "tags": ["scope:shared"],
  },
}
```

Configure the `@nx/enforce-module-boundaries` ESLint rule:
```bash
nx add @nx/eslint-plugin @nx/devkit
```

eslint.config.mjs
```js
import nx from '@nx/eslint-plugin';

export default [
  ...nx.configs['flat/base'],
  ...nx.configs['flat/typescript'],
  ...nx.configs['flat/javascript'],
  {
    files: ['**/*.ts', '**/*.tsx', '**/*.js', '**/*.jsx'],
    rules: {
      '@nx/enforce-module-boundaries': [
        'error',
        {
          allow: [],
          // update depConstraints based on your tags
          depConstraints: [
            {
              sourceTag: 'scope:shared',
              onlyDependOnLibsWithTags: ['scope:shared'],
            },
            {
              sourceTag: 'scope:admin',
              onlyDependOnLibsWithTags: ['scope:shared', 'scope:admin'],
            },
            {
              sourceTag: 'scope:client',
              onlyDependOnLibsWithTags: ['scope:shared', 'scope:client'],
            },
          ],
        },
      ],
    },
  },
];
```

`onlyDependOnLibsWithTags` can be,
- `["*"]` - can depend on any other project
- `string` - allow exact tags
	- `["scope:shared"]`
- `regex` - allow tags matching the regex
	- `["/^scope.*/"]`
- `glob` - allows tags matching globs
	- `["scope:*"]`
- 

## Connections
*How does this idea relate to what I already know?*
- It's an example of ...
- It's the opposite of ...
- It's used in ...
	- [[Module Boundaries]]

## Open Questions
*What does this make me wonder about? What am I still curious about?*
- Q - What are good naming conventions for scopes?
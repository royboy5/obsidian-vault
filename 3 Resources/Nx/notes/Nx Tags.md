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

### Example: Boundaries
https://nx.dev/docs/guides/enforce-module-boundaries/ban-dependencies-with-tags
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

### Other Boundaries
- https://nx.dev/docs/guides/enforce-module-boundaries/ban-external-imports
- https://nx.dev/docs/guides/enforce-module-boundaries/tag-multiple-dimensions

This configuration is the "Security Guard" of your monorepo. It enforces strict rules about which projects are allowed to "talk" (import) to each other.

Without this, any file could import any other file, eventually turning your code into a "Spaghetti Monorepo" where everything depends on everything.

Here is the breakdown of the two "dimensions" you are enforcing: **Scope** (Business Domains) and **Type** (Architecture Layers).

### 1. The "Scope" Rules (Silos)

These rules ensure that different business domains stay separate. You don't want your **Admin** panel code accidentally importing secret logic from the **Client** (Storefront) app.

- **`sourceTag: 'scope:shared'`**
    
    - _Translation:_ "I am a Shared library."
        
    - _Rule:_ I can **only** import other Shared libraries. I cannot touch Admin or Client code.
        
- **`sourceTag: 'scope:admin'`**
    
    - _Translation:_ "I am part of the Admin domain."
        
    - _Rule:_ I can import other **Admin** stuff (my own domain) OR **Shared** stuff. But I strictly **cannot** import `scope:client`.
        
- **`sourceTag: 'scope:client'`**
    
    - _Translation:_ "I am part of the Client domain."
        
    - _Rule:_ I can import **Client** stuff or **Shared** stuff. I **cannot** import `scope:admin`.
        

### 2. The "Type" Rules (The Ladder)

These rules enforce a strict **unidirectional data flow** (Hierarchy). High-level things (Apps) can use low-level things (Utils), but low-level things can _never_ look up.

- **`type:app` (Top of the food chain)**
    
    - Can import: `feature`, `ui`, `util`.
        
    - _Logic:_ The App connects everything together.
        
- **`type:feature` (Smart Components)**
    
    - Can import: `ui`, `util`.
        
    - _Logic:_ A "Login Page" (Feature) uses a "Button" (UI) and "AuthHelper" (Util).
        
    - _Restriction:_ It **cannot** import other Features. (Features should be independent pages that don't know about each other).
        
- **`type:ui` (Dumb Components)**
    
    - Can import: `ui`, `util`.
        
    - _Logic:_ A "Card" (UI) can use a "Button" (UI).
        
    - _Restriction:_ It **cannot** import a Feature. A generic Button should never know about a "Login Page."
        
- **`type:util` (Bottom of the chain)**
    
    - Can import: `util` (only other utils).
        
    - _Logic:_ A pure math helper function should never try to render a React Component (UI) or navigate to a Page (Feature).
        

---

### 3. How they work together (The "AND" Logic)

This is the most critical part. Most libraries in your workspace will have **two tags**: one from Scope and one from Type.

**Example Library:** `libs/admin/feature-dashboard`

- **Tags:** `scope:admin`, `type:feature`
    

When this library tries to import something, **BOTH** sets of rules must pass.

#### Scenario A: Good Import ✅

- **Importing:** `libs/admin/ui-button` (`scope:admin`, `type:ui`)
    
- **Check 1 (Scope):** Does `scope:admin` allow `scope:admin`? **YES.**
    
- **Check 2 (Type):** Does `type:feature` allow `type:ui`? **YES.**
    
- **Result:** Allowed.
    

#### Scenario B: Bad Scope (The "Leak") ❌

- **Importing:** `libs/client/ui-header` (`scope:client`, `type:ui`)
    
- **Check 1 (Scope):** Does `scope:admin` allow `scope:client`? **NO.** (Bouncer stops you here).
    
- **Result:** Blocked. _Error: A project tagged 'scope:admin' can only depend on libs with tags: 'scope:admin', 'scope:shared'._
    

#### Scenario C: Bad Type (The "Circular Dependency Risk") ❌

- **Importing:** `apps/admin-dashboard` (`scope:admin`, `type:app`)
    
- **Check 1 (Scope):** Does `scope:admin` allow `scope:admin`? **YES.**
    
- **Check 2 (Type):** Does `type:feature` allow `type:app`? **NO.** (You can't import the App _into_ a library).
    
- **Result:** Blocked.

---

### Summary

This config creates a rigid grid for your code:

1. **Verticals:** Apps can only talk to their own Domain (`scope`).
    
2. **Horizontals:** Code must flow Down (`app` -> `feature` -> `ui` -> `util`).
## Connections
*How does this idea relate to what I already know?*
- It's an example of ...
- It's the opposite of ...
- It's used in ...
	- [[Module Boundaries]]

## Open Questions
*What does this make me wonder about? What am I still curious about?*
- Q - What are good naming conventions for scopes?



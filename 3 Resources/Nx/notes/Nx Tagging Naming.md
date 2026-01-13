---
created: 2026-01-03 12:26
updated: 2026-01-03 12:26
tags:
  - notes
---

## Nx Tagging Naming

## Core Idea
*This is the single concept, explained in my own words.  This forces me to process the information (encode it) rather than just copy-pasting.*

Here is a summary framework for naming your **Scopes** and **Types**.

Think of your Monorepo as a grid. **Scope** is the vertical slice (Business), and **Type** is the horizontal slice (Tech).

### 1. Naming Scopes (Business Domains)

**Rule:** Name these after **Departments** in your company or **Major Features** of your product.

- **The Question to ask:** "If I delete this folder, what part of the business shuts down?"
    

|**Tag Name**|**Definition**|**Examples**|
|---|---|---|
|`scope:platform`|**The Orchestrator.** Code that ties the whole system together.|Main API (`coaching-api`), Background Worker.|
|`scope:client`|**The Frontends.** Code that runs in a browser/device.|React App, Mobile App, Website.|
|`scope:shared`|**The Foundation.** Code used by _everyone_.|UI Kits, Database Clients, Date Formatters.|
|`scope:[domain]`|**The Features.** Specific business capabilities.|`scope:users`, `scope:chat`, `scope:calendar`, `scope:billing`.|

### 2. Naming Types (Architecture Layers)

**Rule:** Name these after **Technical Responsibility**.

- **The Question to ask:** "Where does this sit in the dependency tree? Top or Bottom?"
    

|**Tag Name**|**Definition**|**Imports...**|
|---|---|---|
|`type:app`|**Deployables.** The actual executable or website.|`feature`, `ui`, `util`|
|`type:feature`|**Smart Logic.** "Smart Components" or "Controllers". Knows about state/data.|`ui`, `util`|
|`type:ui`|**Dumb UI.** Pure presentation. No data fetching. (React only).|`ui`, `util`|
|`type:util`|**Helpers.** Pure functions, constants, or types.|`util`|

### 3. The "Matrix" (How they combine)

Every library gets **one** from column A and **one** from column B.

#### Real-world Examples for your Coaching App:

1. **The Login Page (Frontend)**
    
    - **Name:** `libs/users/feature-login`
        
    - **Tags:** `scope:users`, `type:feature` (It's a smart feature belonging to the Users domain).
        
2. **The "Submit" Button (Shared)**
    
    - **Name:** `libs/shared/ui-kit`
        
    - **Tags:** `scope:shared`, `type:ui` (It's dumb UI used by everyone).
        
3. **The Chat Server (Backend API)**
    
    - **Name:** `apps/coaching-api`
        
    - **Tags:** `scope:platform`, `type:app` (It's the main system orchestrator).
        
4. **The Date Formatter (Shared Helper)**
    
    - **Name:** `libs/shared/util-dates`
        
    - **Tags:** `scope:shared`, `type:util` (It's a helper used by everyone).

## Connections
*How does this idea relate to what I already know?*
- It's an example of ...
- It's the opposite of ...
- It's used in ...

## Open Questions
*What does this make me wonder about? What am I still curious about?*
- Q - How does this apply to ...?
- Q - What is the difference between this and ...?
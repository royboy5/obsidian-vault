---
tags: [project/volleyball-suite, domain/coach, type/hub-note]
role: product
---

# Coach
↩ [[Volleyball Suite]] (canvas)

> Teams, rosters, jersey numbers, stat tracking, and the animated playbook/drawing tool. Second product built (Step 5).

## At a glance
- **Apps:** `coach-web` (React, read-only play viewer) · `coach-api` (Hono) · `mobile-coach` (Flutter, full drawing/animation editor)
- **DB:** `postgres-coach`
- **Roles:** coach (own teams), org_admin/staff (org-wide), player/parent (view only)

## Sub-domains (source docs)
| Area | Covers | Doc |
|---|---|---|
| Teams + Roster + Stats | Team creation, jersey numbers, org-wide player query, stat recording | [[teams]] |
| Playbook | Play/drill data model, animation/keyframes, cross-org sharing, drawing tool platform split | [[playbook]] |

## Key data models
`Team`, `PlayerTeamMembership`, `Stat`, `Playbook`, `Play` (canvasData + keyframes), `PlayShare`

## Notable mechanics
- A `Play` is a timeline, not a snapshot — `keyframes` interpolated at render time
- Full drawing/editing only on **mobile** (Flutter, `packages/canvas`); web is a read-only viewer (full web editor deferred to Step 6)
- Stats link to a `matchId`, which originates from a Club Manager calendar `Event` (type: match) — see [[calendar]]
- `PlayShare` intentionally crosses org boundaries — a coach can share a play with a user in a different org

## Related
[[Club Manager]] (match events feed stats) · [[AI Assistant]] (AI reviews animated plays) · [[Identity Service]]

---
tags: [project/volleyball-suite, concept/multi-tenancy, type/atomic-note]
role: cross-cutting
---

# Multi-Tenancy & RLS
↩ [[Volleyball Suite]] (canvas)

> The core rule the whole suite is built around: every piece of org data is scoped to that org, enforced at the Postgres level in *every* product database — not just checked in application code.

## The mechanism
1. User selects an **active org** → stored as `active_org_id` claim in the Better Auth JWT
2. Every product API's `packages/auth` middleware reads that claim, resolves the membership, and runs `SET LOCAL app.active_org_id = …` before the query
3. Every org-scoped table has an RLS policy that filters on that session variable

```sql
CREATE POLICY "org_isolation" ON events
FOR ALL
USING (
  current_setting('app.active_org_id', true) = 'super_admin'
  OR org_id = current_setting('app.active_org_id')::uuid
);
```

## Role is per-org, never global
`UserOrgMembership.role` — one person can be `coach` at one org and `org_admin` at another, on the *same account*.

```
super_admin (platform, not org-scoped)
  └── org_admin
      └── staff
          └── coach
              └── player
                  └── parent
```

## What deliberately crosses org boundaries
`PlayerProfile` + QR, `PlayShare` (a coach can share a play with a user in another org), tryout registration (any player can register at any org). Everything else — teams, stats, documents, events, playbooks-unless-shared — is hard org-scoped.

## Related
[[Identity Service]] · [[Auth Middleware & Security]] · full pattern → [[shared]]

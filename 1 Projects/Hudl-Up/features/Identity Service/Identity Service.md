---
tags: [project/volleyball-suite, domain/identity, type/hub-note]
role: foundation
---

# Identity Service
↩ [[Volleyball Suite]] (canvas)

> The shared foundation every other product depends on. Not a purchasable product itself — auth, profiles, orgs, memberships, billing entitlements, and platform-level super admin all live here. Nothing else works until this is built (Step 2, before any product).

## At a glance
- **App:** `apps/identity-api` (Hono on Node)
- **DB:** `postgres-identity` (its own Postgres instance)
- **Auth:** Better Auth — JWT + stateful session store, custom claims (`active_org_id`, `platform_role`)
- **Security:** rate limiting (`@hono/rate-limiter`), HaveIBeenPwned password check (k-anonymity), TOTP MFA (optional for players/parents, enforced for org_admins), Cloudflare IP trust + Nginx allowlist

## Owns
- User accounts, `PlayerProfile`, `Organization`, `UserOrgMembership`
- `ParentPlayerLink`, `UserContactPreferences`, `OrgInviteCode`
- `BillingEntitlement` + `SeatAssignment`
- Platform / `super_admin` routes (org suspend, impersonation, analytics, feature flags) — see [[platform]]

## Registration flows (summary)
- **Player** → account → PlayerProfile → QR generated → join org (invite code or search + approval) → membership created
- **Parent** → account (no profile needed) → `ParentPlayerLink` → confirmed by player/org_admin
- **Staff** → invited by org_admin only, never self-registers
- **Org creation** → creator becomes `org_admin`, MFA setup enforced before proceeding

Full flow detail → [[identity]] · [[auth-orgs]]

## Every product depends on this via
- `packages/auth` — JWT validation, session check, role guards → see [[Auth Middleware & Security]]
- `packages/billing` — entitlement checks → see [[Billing Model]]
- `packages/identity` — API client (product APIs never query the identity DB directly)

## Build order
`1a`–`1z` in [[identity]]: Moon workspace → Postgres → Better Auth → JWT claims → rate limiting → HIBP → social auth → orgs → profiles → invite codes → `packages/auth`/`identity`/`billing` → MFA → billing/Stripe → internal routes → platform routes → Cloudflare/Nginx.

## Related
[[Multi-Tenancy & RLS]] · [[Billing Model]] · [[Monorepo & Toolchain]]

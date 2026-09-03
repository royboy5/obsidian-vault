---
tags: [project/volleyball-suite, concept/auth-security, type/atomic-note]
role: cross-cutting
---

# Auth Middleware & Security
↩ [[Volleyball Suite]] (canvas)

> The single shared middleware (`packages/auth`) every product API mounts on `/api/*`, plus the security hardening built into identity-api itself.

## Request flow through every product API
```
JWT validated (signature) → session checked active in identity DB
  → super_admin? bypass org checks
  → else: resolve org membership for active_org_id → 403 if not a member
  → entitlement check (packages/billing) → 402 if none
  → SET LOCAL app.active_org_id (RLS) → route handler
```

## Session model
JWT + **stateful** session store in Postgres (not pure stateless JWT) — revoking a session (logout, suspension) cuts access immediately instead of waiting for token expiry.

## JWT custom claims
`active_org_id` (drives RLS everywhere), `platform_role` (`super_admin` or null)

## Hardening on identity-api
- Rate limiting: login (10/15min per IP+email), registration (5/hr per IP), password reset (3/hr per email)
- HaveIBeenPwned password check via k-anonymity (only first 5 hash chars sent, fails open on outage)
- Cloudflare IP trust + Nginx allowlist (rejects direct hits bypassing Cloudflare)
- TOTP MFA — optional for players/parents, strongly encouraged for coaches, **enforced for org_admins**

## Related
[[Identity Service]] · [[Multi-Tenancy & RLS]] · full code → [[identity]]

---
tags: [project/volleyball-suite, domain/club-manager, type/hub-note]
role: product
---

# Club Manager
↩ [[Volleyball Suite]] (canvas)

> Tryouts, documents & waivers, payments, calendar/events, and QR check-in. First product built end-to-end (Step 4) to prove the full stack works before Coach and AI Assistant are added.

## At a glance
- **Apps:** `club-manager-web` (React + Vite) · `club-manager-api` (Hono) · `mobile-club-manager` (Flutter)
- **DB:** `postgres-club-manager`
- **Roles:** org_admin (create/manage), staff (operations), coach (check-in only), player/parent (register, RSVP, sign)

## Sub-domains (source docs)
| Area | Covers | Doc |
|---|---|---|
| Calendar + Events | Events, RSVP, recurring RRULE, .ics feeds | [[calendar]] |
| Tryouts | Creation, registration, Stripe/cash payment, 5-step check-in, reporting | [[tryouts]] |
| Documents | Waivers, versioning, typed/drawn signatures, legal DocumentSnapshot | [[tryouts]] |
| Communication | Deep links (mailto/sms/whatsapp), Resend email, QR generation/scanning | [[communication]] |
| Org/Staff mgmt | Invite codes, staff onboarding, membership approval | [[auth-orgs]] |

## Key data models
`Event`, `EventRSVP`, `Tryout`, `TryoutRegistration`, `Document`, `DocumentSignature`, `DocumentSnapshot`, `OrgInviteCode` (identity)

## Notable mechanics
- Tryout creation auto-creates a calendar `Event` (type: tryout)
- Check-in is a strict 5-step linear flow: identify → verify payment → verify photo → assign shirt number → complete (atomic save)
- Tryout registration crosses org boundaries by design — any player can register for any org's tryout

## Related
[[Multi-Tenancy & RLS]] · [[Identity Service]] · [[Coach]] (match events created here feed stat tracking)

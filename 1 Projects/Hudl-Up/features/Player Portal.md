---
tags: [project/volleyball-suite, domain/player-portal, type/hub-note]
role: product
---

# Player Portal
↩ [[Volleyball Suite]] (canvas)

> The free, mobile-only consumer layer for players and parents. Always included with any org subscription — no dedicated backend or DB of its own.

## At a glance
- **App:** `mobile-player-portal` (Flutter only — no web app)
- **DB:** none — reads from identity-api + whichever product APIs the org is entitled to
- **Cost:** always free once the org has *any* active subscription

## Feature set unlocks based on the org's subscriptions
| Org has… | Player Portal shows |
|---|---|
| Club Manager | Tryout registration, document signing, RSVP, profile + QR |
| Coach | Stats, shared playbooks, team calendar |
| AI Assistant | Rules lookup, technique coaching, video analysis |
| Full Suite | Everything |

## Why it exists
Grows the user base and stickiness — every org that buys *any* product gives all its players/parents a free portal, which becomes the natural upsell surface for the other products.

## Related
[[Identity Service]] (owns PlayerProfile, QR, auth) · [[Billing Model]]

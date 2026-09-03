---
tags: [project/volleyball-suite, concept/billing, type/atomic-note]
role: cross-cutting
---

# Billing Model
↩ [[Volleyball Suite]] (canvas)

> One shared seat pool per org — a seat buys access to *every* product the org subscribed to, not per-product seats.

## What orgs buy
- Individual products (Club Manager / Coach / AI Assistant), per user/month
- Bundles, per seat/month with volume discount: **Ops** (Club Manager) · **Coaching** (Coach + AI Assistant) · **Full Suite** (all three)
- **Player Portal is always free** once the org has any active subscription

## Entitlement check (`packages/billing`)
```ts
async function hasEntitlement(userId, orgId, product) {
  const entitlement = await getOrgEntitlement(orgId)
  if (!entitlement || entitlement.status !== 'active') {
    return checkIndividualEntitlement(userId, product)
  }
  if (!entitlementCoversProduct(entitlement, product)) return false
  return isSeatAssigned(entitlement.id, userId)
}
```

## Models (identity DB)
`BillingEntitlement` (org OR user, `productIds[]`, `seats`/`seatsUsed`, Stripe IDs, status) · `SeatAssignment` (entitlement ↔ user)

## Related
[[Identity Service]] · [[Player Portal]] · [[Multi-Tenancy & RLS]]

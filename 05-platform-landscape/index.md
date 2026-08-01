---
title: 5. Platform Landscape
nav_order: 6
has_children: true
---

# Stage 5 — Platform Landscape

> *"Don't just install them. Understand why organisations use them."*

## Why this section exists, and why it's deliberately fifth

You cannot work in this field without product knowledge. Every project has products; every interview asks about them; every design must be implementable in *something*.

But product knowledge acquired **before** the conceptual model is vendor-shaped and doesn't transfer. Acquired **after**, it takes weeks instead of months, and every new product you meet is just a new vocabulary for structures you already understand.

{: .concept }
> **Learn the concept, then learn each product as a dialect.** An "Identity Cube" (SailPoint), a "Person object" (One Identity Manager), an "identity profile" (Ping) and a "user object" (Entra) are the same correlated record. Once you see that, evaluating a new platform becomes: *what does it call the things I already know about, what can it do that's genuinely distinctive, and where will it fight me?*

## Pages in this stage

| # | Page | Contents |
|:--|:--|:--|
| 0 | [The Vendor-Neutral Map](00-vendor-neutral-map.md) | **Start here.** The Rosetta stone: same concepts, every vendor's vocabulary |
| 1 | [SailPoint](01-sailpoint.md) | The IGA market reference — IdentityIQ and Identity Security Cloud |
| 2 | [One Identity](02-one-identity.md) | One Identity Manager, Active Roles, Safeguard — deep enterprise IGA |
| 3 | [Ping Identity](03-ping-identity.md) | PingFederate, PingOne, PingAM/PingDirectory (incl. the ForgeRock line) |
| 4 | [Okta & Microsoft Entra ID](04-okta-and-entra.md) | The two dominant access-management platforms |
| 5 | [Saviynt, ForgeRock & Keycloak](05-saviynt-forgerock-keycloak.md) | The rest of the field worth knowing |
| 6 | [PAM Vendors](06-pam-vendors.md) | CyberArk, Delinea, BeyondTrust, Safeguard, Vault |
| 7 | [Choosing a Platform](07-choosing-a-platform.md) | Requirements, RFPs, PoCs, TCO, and how selections go wrong |

## How to read the vendor pages

Each one covers: **what it is, the architecture, the object model (mapped to concepts), what it does genuinely well, where it's hard, and who it fits.** They deliberately avoid:

- Feature checklists (they change quarterly and are on the vendor's site).
- Version numbers and screenshots (stale within months).
- A verdict on which is "best" (the answer is always "for what, in whose estate, with which team?").

{: .warning }
> **Everything product-specific here will drift.** Vendors rename products, acquire each other, move on-prem capabilities to SaaS and deprecate modules. ForgeRock is now part of Ping. Azure AD is now Entra ID. Thycotic and Centrify became Delinea. **Verify current details against vendor documentation before relying on them in a design or a procurement.** The durable content is the conceptual mapping — that ages far more slowly than any feature list.

## One more thing about hands-on

The infographic advice — *get hands-on with IAM platforms* — is right, with one refinement: **go deep in one, literate in the rest.**

Depth in one product teaches you how a real implementation behaves: what's native versus customised, where the sharp edges are, what "supported" actually means. Literacy in the others means you can read their documentation, size an integration and hold a credible conversation.

Pick your one based on your [specialisation track](../00-start-here/02-the-iam-architect-role.md#specialisation-tracks), not on which has the best free tier. See [labs on a zero budget](../09-practice/05-labs-on-a-budget.md) for how to get hands-on without a licence.

---

**Start:** [The Vendor-Neutral Map](00-vendor-neutral-map.md) →

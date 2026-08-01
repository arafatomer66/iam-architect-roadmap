---
title: Saviynt, ForgeRock & Keycloak
parent: 5. Platform Landscape
nav_order: 5
---

# Saviynt, ForgeRock, Keycloak & the Rest

*Verify current details against vendor documentation.*

---

## Saviynt

**What it is:** a cloud-native IGA platform, with strong application GRC and cloud entitlement coverage. The main SaaS-native challenger to SailPoint in enterprise IGA.

**Distinctive strengths:**

- **Cloud-first architecture** — no on-premises platform to run; agents/gateways reach on-prem targets.
- **Application GRC** — deep SoD and risk analysis inside major business applications (SAP, Oracle, Workday), overlapping territory traditionally held by dedicated GRC tools. For SAP-heavy organisations this can consolidate two products into one.
- **CPAM** — cloud privileged access, addressing JIT elevation for cloud roles rather than only vaulting server passwords.
- **Analytics-led** — peer analytics, risk scoring and recommendations woven into request and certification flows.

**Where it's hard:** a younger ecosystem than SailPoint (fewer experienced implementers, though growing); SaaS means less control over upgrade timing; connector depth for very old on-premises systems varies — test your hardest targets in a PoC.

**Fits:** cloud-forward enterprises wanting IGA without running the platform; SAP-heavy estates wanting application GRC and IGA together; organisations replacing a legacy on-prem IGA and unwilling to run another one.

---

## ForgeRock (now part of Ping Identity)

**What it was:** a full-stack, self-hostable identity platform — Access Management (AM), Identity Management (IDM), Directory Services (DS) and Identity Gateway (IG) — popular with organisations wanting deep customisation, container deployment and no dependence on a vendor's SaaS.

**What it is now:** part of [Ping](03-ping-identity.md), with the portfolios converging. The components are being positioned as PingAM, PingIDM, PingDS and PingGateway.

**Why it still matters:**

- A large installed base — you will meet it in CIAM and telco/finance estates.
- **PingDS/DJ (OpenDJ lineage)** is a serious directory, and PingIG/OpenIG remains a widely used identity gateway.
- Its **authentication trees/journeys** model (composable, node-based authentication flows) was influential, and the same idea now appears across the industry, including in DaVinci.
- Kubernetes-native deployment was a genuine differentiator for organisations wanting to own their identity platform.

{: .warning }
> **If you are running ForgeRock, roadmap clarity is now a procurement question, not a technical one.** Ask Ping directly, in writing, about the support timeline and convergence path for the specific components you run, and factor a potential migration into your multi-year plan. This is a general lesson worth internalising: **vendor consolidation is a standing architectural risk in identity**, and "what happens if this vendor is acquired?" belongs in every selection.

---

## Keycloak

**What it is:** the leading open-source identity and access management server (a CNCF-adjacent project, originally from Red Hat, commercially supported as Red Hat build of Keycloak). SAML, OIDC, OAuth 2.x, user federation to LDAP/AD, identity brokering, fine-grained authorisation services, and a full admin API.

**Why an architect should care, even if you'd never deploy it in production:**

- **It is the best learning platform in existence for identity protocols.** Every setting is visible and changeable; you can watch a full SAML or OIDC flow and break it deliberately. See [labs](../09-practice/05-labs-on-a-budget.md).
- It's a legitimate production choice where there's engineering capacity — common in product companies, public sector and cost-sensitive environments.
- It's frequently embedded in other products, so you'll meet it indirectly.

**The honest trade-off:** no licence cost, and **you are the vendor**. You own high availability, upgrades, security patching, capacity planning and 3am incidents for the system that authenticates everyone. That total cost is frequently higher than a SaaS licence for organisations without a platform team — and lower for those with one. Make that comparison explicitly rather than assuming "free" means cheap.

---

## Others worth knowing by name

| Vendor / product | Category | Note |
|:--|:--|:--|
| **Omada Identity** | IGA | Strongly templated, best-practice-driven; popular in Microsoft-centric European estates |
| **IBM Security Verify** | IGA + AM | Large installed base, particularly in existing IBM estates |
| **Oracle Identity Governance / Access Manager** | IGA + AM | Legacy-heavy installed base; you'll meet it in migration projects |
| **NetIQ / OpenText Identity Manager** | IGA | Micro Focus/OpenText lineage; present in older estates |
| **Okta / Entra governance modules** | IGA-lite | Covered in [the previous page](04-okta-and-entra.md) |
| **Auth0** | CIAM | Now Okta Customer Identity Cloud |
| **Curity, FusionAuth, Ory, Zitadel, Authentik, Logto** | AM / CIAM | Developer-oriented alternatives; Ory and Zitadel notable in cloud-native estates |
| **Silverfort** | Identity security | MFA and protection for systems that can't natively support it (legacy, service accounts) — a genuinely useful pattern for hard-to-modernise estates |
| **BeyondIdentity, HYPR, Transmit Security** | Passwordless / CIAM | Specialists in phishing-resistant authentication and orchestration |
| **Veza, Permiso, Push Security** | Identity security / entitlements | The newer CIEM/ITDR/NHI generation |
| **SGNL, PlainID, Axiomatics, Styra, Cerbos, Oso** | Authorisation | Externalised, fine-grained authorisation |
| **Astrix, Entro, Oasis, Clutch** | NHI security | The emerging non-human identity category |

{: .architect }
> **Two structural observations to carry into any selection.** First, **consolidation is relentless** — ForgeRock into Ping, Thycotic and Centrify into Delinea, Auth0 into Okta. Any vendor you choose may be acquired within your architecture's lifetime, so favour standards-based integration and keep an exit path. Second, **new categories emerge where the incumbents' assumptions break** — CIEM exists because IGA couldn't handle computed cloud permissions; NHI security exists because JML assumes humans; ITDR exists because SIEM didn't understand identity. Watching *which assumption a new category is attacking* is the fastest way to understand where your own architecture is weak.

---

**Next:** [PAM Vendors](06-pam-vendors.md) →

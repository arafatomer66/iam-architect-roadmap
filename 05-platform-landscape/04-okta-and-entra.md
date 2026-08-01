---
title: Okta & Microsoft Entra ID
parent: 5. Platform Landscape
nav_order: 4
---

# Okta & Microsoft Entra ID

*Verify current product details against vendor documentation — both platforms release continuously.*

The two dominant access-management platforms. Most organisations end up with one as their workforce IdP.

---

## Microsoft Entra ID

### What it is

The identity service underlying Microsoft 365 and Azure, and by installed base the most widely deployed enterprise IdP in the world — often because it arrived with the productivity licence rather than because it was selected.

**It is not Active Directory.** Flat rather than hierarchical, Graph API rather than LDAP, OIDC/SAML rather than Kerberos. See [Active Directory](../01-it-fundamentals/02-active-directory.md) for the comparison that most often causes confusion.

### The pieces that matter

| Component | Purpose |
|:--|:--|
| **Users, groups, administrative units** | The directory model |
| **App registrations / service principals** | Application identity; delegated vs application permissions |
| **Conditional Access** | The policy engine: signals → controls. **Where Zero Trust actually gets implemented** in Microsoft estates |
| **Identity Protection** | Risk detection (leaked credentials, anomalous sign-in, risky users) |
| **PIM** | Just-in-time, approval-gated activation of privileged roles — native PAM for directory and Azure roles |
| **Entra ID Governance** | Access packages, access reviews, lifecycle workflows — native IGA |
| **Entra Connect / Cloud Sync** | Hybrid synchronisation with on-prem AD |
| **External ID** | B2B guests and CIAM (succeeding Azure AD B2C) |
| **Verified ID** | Verifiable credentials |
| **Workload identities** | Managed identities and workload identity federation |

### Strengths

**Integration with the Microsoft estate** — Office, Windows, Intune, Defender, Azure. Device compliance signals from Intune feeding Conditional Access is a genuinely strong Zero Trust foundation that other vendors must integrate to reach.

**Conditional Access** is a mature, expressive policy engine, and the signals available to it (device state, sign-in risk, user risk, location, application, session context) are broad.

**Licensing gravity.** If you already pay for E5, much of this is included, which is a powerful economic argument that frequently overrides technical preference.

**PIM** is a genuinely good JIT model for directory and Azure roles.

### Limitations to be honest about

- **Non-Microsoft coverage** is decent and not equal to a dedicated IdP's ecosystem; complex federation scenarios can require workarounds.
- **Entra ID Governance** is capable and is not yet equivalent to SailPoint or One Identity Manager for complex, heterogeneous, heavily-audited estates. **The trap is assuming it is, because it's already licensed.** Test it against your hardest governance requirement — mainframe, SAP, cross-application SoD, evidence generation — before deciding.
- **Hybrid complexity** — Entra plus AD means two directories and a sync relationship, permanently ([directory sync](../02-identity-fundamentals/21-directory-sync.md)).
- **Tenant-level blast radius** — a Global Administrator compromise is total. Conditional Access misconfiguration can lock everyone out, which is why break-glass accounts excluded from CA are mandatory, not optional.

---

## Okta

### What it is

An independent, cloud-first identity platform, split into:

- **Workforce Identity Cloud** — SSO, MFA (Okta Verify, FastPass), Universal Directory, Lifecycle Management (provisioning), Workflows (no-code automation), Identity Governance, Privileged Access.
- **Customer Identity Cloud (Auth0)** — the developer-oriented CIAM platform, with Actions/Rules for extensibility.

### The concepts

| Concept | Meaning |
|:--|:--|
| **Universal Directory** | The user store, with flexible profiles and attribute mapping |
| **Applications** | Integrations, largely from a very large pre-built catalogue |
| **Groups & group rules** | Entitlement model; rules assign membership dynamically |
| **Sign-on policies** | Authentication requirements, global and per application |
| **Authenticators / FastPass** | MFA methods, including phishing-resistant device-bound options |
| **Lifecycle Management** | Inbound (HR) and outbound (SCIM) provisioning |
| **Workflows** | Visual automation — the flexible glue for edge cases |
| **Org2Org** | Okta-to-Okta federation, used for M&A and delegated tenancy |

### Strengths

**Speed of application onboarding.** The integration catalogue is the largest in the market, and for a SaaS-heavy organisation this converts weeks of integration work into hours.

**Neutrality.** Not tied to one productivity ecosystem, which matters in mixed Microsoft/Google/AWS estates.

**Developer experience** (especially Auth0) — good documentation, SDKs and extensibility, which is why it dominates in product companies.

**Workflows** covers the long tail of "we need this one odd thing" without custom code.

### Limitations

- **Cost at scale**, and feature tiering — governance, privileged access and advanced MFA are separate SKUs, so the entry price is not the running price.
- **Governance depth** — Okta Identity Governance is improving and, like Entra's, is not yet equivalent to a dedicated IGA platform for complex regulated estates.
- **Another vendor to secure.** Okta's own security incidents have been instructive for the whole industry: they demonstrated that **your IdP vendor's support and subprocessor access is part of your attack surface**. Ask specifically how vendor support accesses your tenant, and configure tenant-level hardening accordingly ([securing IAM](../04-architecture-practice/06-securing-iam.md)).

---

## Choosing between them

| If… | Lean |
|:--|:--|
| The estate is Microsoft-centric, E5 is licensed, devices are Intune-managed | **Entra ID** |
| The estate is genuinely mixed, or SaaS-first with many non-Microsoft apps | **Okta** |
| You need the deepest device-compliance-driven Conditional Access | **Entra ID** |
| You need the fastest onboarding of a long tail of SaaS | **Okta** |
| You're building a customer-facing product and want developer-first CIAM | **Okta CIC (Auth0)** |
| You need complex multi-party federation and protocol bridging | **Neither — look at [Ping](03-ping-identity.md)** |
| You need enterprise-grade governance over a heterogeneous estate | **Neither alone — pair with a dedicated [IGA](01-sailpoint.md)** |

{: .architect }
> **The most consequential decision here is rarely "Okta or Entra" — it's whether the native governance module is sufficient.** Both vendors have credible, improving governance offerings, and for a mid-size organisation with a homogeneous SaaS estate they are frequently the right answer: one platform, one licence, no integration. For a regulated enterprise with a mainframe, SAP, 200 applications, cross-application SoD and an auditor who wants evidence, they are usually not — yet. **Test against your hardest governance requirement, not your easiest**, and get the answer in writing before the licence is signed. This single question changes programme cost by seven figures in large organisations.

---

## What an architect should be able to say

- Entra ID is **not** AD; hybrid means both, permanently, with a sync relationship to operate.
- Conditional Access is Entra's policy engine; sign-on policies are Okta's. Both need break-glass exclusions.
- PIM (Entra) is JIT for directory and Azure roles — use it rather than standing Global Admin.
- Okta's catalogue and Workflows are its practical differentiators; Auth0 is its CIAM line.
- Both offer governance modules that are improving and not yet peers of dedicated IGA for complex estates.
- Both are Tier 0 systems whose vendor support access is part of your threat model.

---

**Next:** [Saviynt, ForgeRock & Keycloak](05-saviynt-forgerock-keycloak.md) →

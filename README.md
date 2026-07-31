---
title: About this repo
nav_exclude: true
search_exclude: true
---

# How to Become an IAM Architect

> A concept-first, vendor-neutral roadmap to Identity & Access Management architecture.
> **Read it as a site → [arafatomer66.github.io/iam-architect-roadmap](https://arafatomer66.github.io/iam-architect-roadmap)**

Most advice on becoming an IAM architect is a product recommendation in disguise — *"learn SailPoint"*, *"get certified in Okta"*. That produces excellent operators and very few architects.

This repo teaches the other thing: **how identity actually works, why organisations grant access, what happens when they don't revoke it, and how to design systems that survive audits, acquisitions, outages and a decade of drift.** Products appear only as dialects of concepts you already understand.

## What's inside

**86 pages across 11 sections.** No build step, no dependencies, no product licence required.

| # | Section | Contents |
|:--|:--|:--|
| — | **[Start Here](00-start-here/)** | What IAM is · the architect role · how to use this · skills matrix & self-assessment · 12-month plan |
| 1 | **[IT Fundamentals](01-it-fundamentals/)** | Directory services & LDAP · Active Directory · networking & DNS · cryptography · PKI/TLS · Windows & Linux admin · cloud · data modelling · HTTP & APIs |
| 2 | **[Identity Fundamentals](02-identity-fundamentals/)** | Authentication · Kerberos/NTLM · SAML · OAuth 2.x · OIDC · tokens & JWT · SCIM · MFA & passwordless · FIDO2/passkeys · sessions & logout · federation & trust · RBAC/ABAC/ReBAC/PBAC · policy as code · joiner-mover-leaver · IGA · role modelling & mining · SoD & certification · PAM · identity data quality · identity proofing · directory sync |
| 3 | **[Identity Domains](03-identity-domains/)** | Workforce · CIAM · B2B & partner · non-human identities & secrets · workload/cloud identity · OT, IoT & edge |
| 4 | **[Architecture Practice](04-architecture-practice/)** | Architectural thinking · reference blueprints · integration patterns · migration & coexistence · HA/DR & scale · securing the IAM platform itself · anti-patterns · diagrams & ADRs |
| 5 | **[Platform Landscape](05-platform-landscape/)** | The vendor-neutral Rosetta stone · SailPoint · One Identity · Ping Identity · Okta & Entra ID · Saviynt/ForgeRock/Keycloak · PAM vendors · how to choose |
| 6 | **[Business & Risk](06-business-and-risk/)** | Business alignment · identity risk · compliance (SOX, GDPR, NIS2, DORA, PCI, HIPAA, ISO, NIST) · operating model · metrics & KPIs · business case & TCO · stakeholders |
| 7 | **[Delivery & Operations](07-delivery/)** | Programme delivery · discovery & assessment · requirements · testing · run & operations · identity incident response |
| 8 | **[The Frontier](08-frontier/)** | Zero Trust & continuous access · ITDR · AI agents & agentic identity · decentralised identity & verifiable credentials · post-quantum & crypto agility |
| 9 | **[Practice](09-practice/)** | Worked case studies · design exercises · whiteboard scenarios · interview prep · labs on a zero budget |
| 10 | **[Reference](10-reference/)** | Glossary (250+ terms) · protocol cheat sheets · standards & RFC index · certifications · templates · reading list |

## The design rule

Every topic is taught in three passes:

1. **The concept** — what problem exists in the world that made someone invent this.
2. **The mechanics** — how it actually works, at the level of detail an architect must hold in their head.
3. **The architect's lens** — the decisions, trade-offs, failure modes and the checklist you must be able to answer before signing off a design.

Vendor material lives in **one section** (Stage 5) and in clearly-marked *"In the products"* callouts elsewhere — so it can go stale without rotting the concepts around it. SailPoint, One Identity Manager and Ping Identity get the deepest treatment because they dominate real enterprise IGA and access-management estates.

## Why concept-first

Products get acquired, renamed and end-of-lifed. ForgeRock is now part of Ping. Azure AD is now Entra ID. The product you'll deploy in 2030 doesn't have a name yet.

What doesn't change:

- Authentication proves *who*; authorisation decides *what*; governance proves it was *right*.
- Identity data has an authoritative source, or it has chaos.
- Access must have an expiry — grants are easy, revocation is the architecture.
- Every credential is a liability with a benefit attached.
- User experience is a security control; a control people route around protects nothing.

## Local preview (optional)

The site is plain Markdown + Jekyll ([just-the-docs](https://just-the-docs.com)), built by GitHub Pages. To preview locally:

```bash
gem install bundler jekyll
bundle exec jekyll serve
```

Or just read the Markdown directly in the repo — every page is written to work in both places, and Mermaid diagrams render natively on GitHub.

## Licence

[CC BY 4.0](LICENSE) — use it, teach from it, adapt it, credit it.

Independent educational project. Trademarks belong to their owners; nothing here is endorsed by any vendor. Verify product specifics against current vendor documentation before relying on them.

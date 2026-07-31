---
title: About this repo
nav_exclude: true
search_exclude: true
---

<div align="center">

<br>

# 🔐 How to Become an **IAM Architect**

### *Not "learn SailPoint." Not "get certified in Okta."*

**A concept-first, vendor-neutral roadmap through the principles, protocols and business thinking that actually make someone an identity architect.**

<br>

[![Read the site](https://img.shields.io/badge/📖_Read_the_site-arafatomer66.github.io-6366f1?style=for-the-badge&labelColor=0b1120)](https://arafatomer66.github.io/iam-architect-roadmap)
[![Pages](https://img.shields.io/badge/86-pages-22d3ee?style=for-the-badge&labelColor=0b1120)](https://arafatomer66.github.io/iam-architect-roadmap)
[![Glossary](https://img.shields.io/badge/250+-glossary_terms-34d399?style=for-the-badge&labelColor=0b1120)](10-reference/01-glossary.md)
[![Licence](https://img.shields.io/badge/CC_BY_4.0-licence-fbbf24?style=for-the-badge&labelColor=0b1120)](LICENSE)

<br>

`SAML` · `OAuth 2.x` · `OIDC` · `SCIM` · `Kerberos` · `FIDO2` · `RBAC` · `ABAC` · `ReBAC`
`Joiner-Mover-Leaver` · `IGA` · `SoD` · `PAM` · `NHI` · `Zero Trust` · `ITDR` · `Agentic identity`

<br>

</div>

---

## The premise

An aspiring IAM professional asks how to become an architect. The usual answer is a product name.

It's the wrong answer.

| ❌ What doesn't work | ✅ What actually makes an architect |
|:--|:--|
| "Learn SailPoint." "Get certified in Okta." | Knowing **who is authoritative** for this data — and who wins on conflict |
| Teaches a console, not a model | Knowing **why a permission exists**, and when it must end |
| Doesn't transfer when the product changes | Knowing what happens on grant, on revoke, and on **the day nobody revoked** |
| Bends every problem toward one tool | Being able to defend the design to **auditors, engineers and a CFO** |

Identity isn't a technology problem. It's understanding how businesses work, how users access applications, why permissions exist, and what happens when access is granted — or not revoked.

This repo teaches that. Products appear as **dialects of concepts you already understand**, isolated in clearly-marked callouts and one dedicated section, so they can go stale without rotting everything around them.

---

## 🗺️ The path

<table>
<tr><th width="60">Stage</th><th>Section</th><th>What it teaches</th><th width="70">Pages</th></tr>

<tr><td align="center">—</td><td><b><a href="00-start-here/">Start Here</a></b></td>
<td>What IAM actually is · the architect role · reading paths · skills matrix & self-assessment · the 12-month plan</td><td align="center">5</td></tr>

<tr><td align="center"><b>1</b></td><td><b><a href="01-it-fundamentals/">IT Fundamentals</a></b></td>
<td>Directory services & LDAP · Active Directory · networking & DNS · cryptography · PKI & TLS · Windows/Linux · cloud IAM · data modelling · HTTP & APIs</td><td align="center">9</td></tr>

<tr><td align="center"><b>2</b></td><td><b><a href="02-identity-fundamentals/">Identity Fundamentals</a></b></td>
<td>Authentication · Kerberos · SAML · OAuth 2.x · OIDC · JWT · SCIM · MFA · FIDO2 & passkeys · sessions · federation · RBAC/ABAC/ReBAC/PBAC · policy as code · JML · IGA · role mining · SoD & certification · PAM · data quality · proofing · directory sync</td><td align="center">21</td></tr>

<tr><td align="center"><b>3</b></td><td><b><a href="03-identity-domains/">Identity Domains</a></b></td>
<td>Workforce · CIAM · B2B & partner · non-human identities & secrets · workload identity · OT, IoT & edge</td><td align="center">6</td></tr>

<tr><td align="center"><b>4</b></td><td><b><a href="04-architecture-practice/">Architecture Practice</a></b></td>
<td>Architectural thinking · reference blueprints · integration patterns · migration & coexistence · HA/DR & scale · securing IAM itself · anti-patterns · diagrams & ADRs</td><td align="center">8</td></tr>

<tr><td align="center"><b>5</b></td><td><b><a href="05-platform-landscape/">Platform Landscape</a></b></td>
<td>The vendor-neutral Rosetta stone · SailPoint · One Identity · Ping Identity · Okta & Entra ID · Saviynt/ForgeRock/Keycloak · PAM vendors · how to choose</td><td align="center">8</td></tr>

<tr><td align="center"><b>6</b></td><td><b><a href="06-business-and-risk/">Business & Risk</a></b></td>
<td>Business alignment · identity risk · compliance (SOX, GDPR, NIS2, DORA, PCI, HIPAA, ISO, NIST) · operating model · metrics & KPIs · business case & TCO · stakeholders</td><td align="center">7</td></tr>

<tr><td align="center"><b>7</b></td><td><b><a href="07-delivery/">Delivery & Operations</a></b></td>
<td>Programme delivery · discovery & assessment · requirements · testing · run & operations · identity incident response</td><td align="center">6</td></tr>

<tr><td align="center"><b>8</b></td><td><b><a href="08-frontier/">The Frontier</a></b></td>
<td>Zero Trust & continuous access · ITDR · AI agents & agentic identity · decentralised identity & verifiable credentials · post-quantum & crypto agility</td><td align="center">5</td></tr>

<tr><td align="center"><b>9</b></td><td><b><a href="09-practice/">Practice</a></b></td>
<td>Worked case studies · design exercises · whiteboard scenarios · interview prep · labs on a zero budget</td><td align="center">5</td></tr>

<tr><td align="center"><b>10</b></td><td><b><a href="10-reference/">Reference</a></b></td>
<td>Glossary (250+ terms) · protocol cheat sheets · standards & RFC index · certifications · templates · reading list</td><td align="center">6</td></tr>
</table>

---

## 🧭 Where do I start?

<details open>
<summary><b>Click to expand the reading paths</b></summary>

<br>

| If you are… | Start here |
|:--|:--|
| 📚 **New to IAM entirely** | [The 12-month learning plan](00-start-here/05-learning-plan.md) — month by month, in dependency order |
| 🛠️ **An IAM engineer moving up** | [Self-assessment](00-start-here/04-self-assessment.md) → [Architecture Practice](04-architecture-practice/) → [Business & Risk](06-business-and-risk/) |
| 💻 **A developer who knows OAuth but not enterprise lifecycle** | [Joiner-Mover-Leaver](02-identity-fundamentals/14-joiner-mover-leaver.md) → [IGA](02-identity-fundamentals/15-identity-governance.md) |
| 🛡️ **From security or GRC** | [Stage 1](01-it-fundamentals/) for mechanism depth, then the protocols in order |
| 🎤 **Interviewing in two weeks** | [Interview prep](09-practice/04-interview-prep.md) → [whiteboard scenarios](09-practice/03-whiteboard-scenarios.md) |
| 🔍 **Choosing or replacing a platform** | [Vendor-neutral map](05-platform-landscape/00-vendor-neutral-map.md) → [How to choose](05-platform-landscape/07-choosing-a-platform.md) |

</details>

---

## 📐 How every page is built

> **1 · The concept** — what problem in the world made someone invent this. A protocol without its problem is trivia.
>
> **2 · The mechanics** — how it works, at the depth an architect must carry without notes.
>
> **3 · The architect's lens** — trade-offs, failure modes, and an **Architect's checklist**: the questions you must be able to answer before signing off a design in that area.

Callouts used throughout:

| | |
|:--|:--|
| 🟣 **Core concept** | The idea to remember if you remember nothing else |
| 🟢 **Architect's lens** | A judgement call, trade-off or war-story-shaped warning |
| 🟡 **In the products** | How SailPoint / One Identity / Ping / Okta / Entra express it |
| 🔴 **Watch out** | A common, expensive mistake |

---

## 🧱 Why concept-first

Products get acquired, renamed and end-of-lifed. ForgeRock is now part of Ping. Azure AD is now Entra ID. The platform you'll deploy in 2030 doesn't have a name yet.

What doesn't change:

```
01 · Authentication proves WHO. Authorisation decides WHAT. Governance proves it was RIGHT.
02 · Identity data has an authoritative source, or it has chaos.
03 · Access must have an expiry — granting is easy, revocation is the architecture.
04 · Every credential is a liability with a benefit attached.
05 · User experience is a security control; a control people route around protects nothing.
```

---

## 🔧 Running it locally

The site is plain Markdown + Jekyll ([just-the-docs](https://just-the-docs.com)), built automatically by GitHub Pages — **no build step required to contribute.**

```bash
git clone https://github.com/arafatomer66/iam-architect-roadmap.git
cd iam-architect-roadmap
bundle exec jekyll serve      # optional local preview
```

Or just read the Markdown in the repo — every page is written to work in both places, and Mermaid diagrams render natively on GitHub.

---

## 🤝 Contributing

Corrections, missing concepts, better explanations and anonymised case studies are all welcome. Product marketing, console screenshots and step-by-step vendor configuration are not — see [CONTRIBUTING.md](CONTRIBUTING.md) for the house style.

## 📄 Licence

[**CC BY 4.0**](LICENSE) — use it, teach from it, adapt it, credit it.

<div align="center">
<br>
<sub>Independent educational project. Trademarks belong to their owners; nothing here is endorsed by any vendor.<br>
Verify product specifics against current vendor documentation before relying on them in a design.</sub>
<br><br>
<b>The tools will change. The principles won't.</b>
</div>

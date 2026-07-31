---
title: How to Use This Repo
parent: Start Here
nav_order: 3
---

# How to Use This Repo

## The structure of every page

Substantive pages follow the same three-pass shape, deliberately:

1. **The concept** — what problem in the world made someone invent this. If you can't state the problem, you don't understand the solution; you've memorised it.
2. **The mechanics** — how it works, at the depth an architect must carry without notes. Enough to design with, not enough to replace the spec.
3. **The architect's lens** — trade-offs, failure modes, and an **Architect's checklist** of questions you should be able to answer before signing off a design in this area.

Vendor-specific material is isolated in `{: .vendor }` **In the products** callouts and in [Stage 5](../05-platform-landscape/), so it can go stale without rotting the durable content around it.

---

## Reading paths

Pick the one that matches you. All of them converge eventually.

### Path A — "I'm new to IAM entirely"

Twelve months, roughly in this order. Full detail in [the learning plan](05-learning-plan.md).

```
Start Here (all)
  → Stage 1: IT Fundamentals (all 9)
  → Stage 2: Identity Fundamentals (all 21 — this is the core; take your time)
  → Stage 3: pick your domain, skim the rest
  → Stage 5: vendor-neutral map only, then ONE product hands-on
  → Stage 4: Architecture Practice
  → Stage 6: Business & Risk
  → Stages 7–9
```

**Do not** open a product trial before you finish Stage 2. The single biggest waste of a beginner's time is learning a console before learning the model it implements — you end up with vendor-shaped knowledge that doesn't transfer.

### Path B — "I'm an IAM engineer moving up"

You likely have Stages 1–2 and one product. Your gaps are almost always the same two:

```
04-self-assessment  (be honest)
  → Stage 4: Architecture Practice (all 8) ← the real gap
  → Stage 6: Business & Risk (all 7)       ← the other real gap
  → Stage 2: fill holes revealed by the assessment
       (usually: ABAC/ReBAC, policy-as-code, role mining, identity data quality)
  → Stage 5: 00-vendor-neutral-map (learn the other dialects)
  → Stage 9: whiteboard scenarios
```

### Path C — "I'm a developer / API person"

You know OAuth and OIDC better than most IAM people. You don't know enterprise lifecycle at all.

```
Stage 2: 14-joiner-mover-leaver, 15-iga, 16-role-modelling,
         17-sod-and-certification, 19-identity-data-quality
  → Stage 1: 01-directory-services, 02-active-directory (yes, really)
  → Stage 3: 04-non-human-identities, 05-workload-identity
  → Stage 6: compliance & operating model
  → Stage 4: integration patterns, anti-patterns
```

### Path D — "I'm from security / GRC"

You have the risk and control language. You need mechanism depth so engineers stop discounting you.

```
Stage 1 (all — especially crypto, PKI, directories)
  → Stage 2: protocols in order (03 → 11), then governance (14 → 21)
  → Stage 3: 04-non-human-identities
  → Stage 8: ITDR, Zero Trust
  → Stage 4: securing the IAM platform, anti-patterns
```

### Path E — "I have an interview in two weeks"

```
09-practice/04-interview-prep
  → 09-practice/03-whiteboard-scenarios
  → 10-reference/02-protocol-cheatsheets
  → 05-platform-landscape/00-vendor-neutral-map
  → backfill whatever the above exposed
```

### Path F — "I'm choosing or replacing a platform"

```
05-platform-landscape/00-vendor-neutral-map
  → 05-platform-landscape/07-choosing-a-platform
  → 04-architecture-practice/04-migration-and-coexistence
  → 06-business-and-risk/06-business-case
  → the relevant vendor pages
```

---

## How to actually learn this (not just read it)

Reading IAM material produces a comfortable illusion of competence. Three habits break it:

**1. Draw it before you read it.** Before each protocol page, sketch what you *think* the flow is. Compare afterwards. The gap between your sketch and reality is the actual learning.

**2. Teach it in six sentences.** After each page, write a six-sentence explanation for a colleague with no IAM background. If you can't, you've absorbed vocabulary, not understanding.

**3. Attach it to a real system.** Every concept should get anchored to something you can point at — your own company's SSO, a free tier, a Keycloak container. Abstract IAM knowledge evaporates in about six weeks. See [labs on a zero budget](../09-practice/05-labs-on-a-budget.md).

{: .note }
> A useful forcing function: keep a running document of **decisions you'd make differently now**. Architects are made by revisited decisions, and you can simulate this by writing down your position on things like "should we use RBAC or ABAC for this" early, then reviewing it after Stage 4.

---

## Depth calibration

Not everything deserves equal depth. As a rough guide for what "knowing it" means:

| Topic | Required depth |
|:--|:--|
| SAML, OAuth 2.x, OIDC, SCIM | **Design without notes.** Draw the flows, name the parameters, list the attack surfaces |
| Kerberos, LDAP, NTLM | **Explain and troubleshoot.** Know why it breaks, not every RFC detail |
| JML, IGA, SoD, certification | **Design without notes.** This is the enterprise bread and butter |
| RBAC / ABAC / ReBAC / PBAC | **Design without notes**, including when each is wrong |
| PKI, TLS, crypto | **Reason correctly.** You must never make a naive crypto decision; you needn't be a cryptographer |
| Cloud IAM (AWS/Azure/GCP) | **Model-level fluency** in at least one, conceptual in the others |
| Product specifics | **Enough to scope, not to configure** — except in your one hands-on product |
| Compliance frameworks | **Know which controls touch identity** and what evidence they demand |

---

## Conventions used here

| Marker | Meaning |
|:--|:--|
| {: .concept } **Core concept** | The idea that, if you only remember one thing, remember this |
| {: .architect } **Architect's lens** | A judgement call, trade-off or war-story-shaped warning |
| {: .vendor } **In the products** | How SailPoint / One Identity / Ping / Okta / Entra / others express it. Verify against current docs |
| {: .warning } **Watch out** | A common, expensive mistake |
| **Architect's checklist** | End-of-page questions you should be able to answer before signing off a design |

Mermaid diagrams render both on GitHub and on the published site. Relative links work in both places too, so you can read the whole thing offline in the repo if you prefer.

---

**Next:** [Self-Assessment & Skills Matrix](04-self-assessment.md) →

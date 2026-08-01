---
title: Discovery & Assessment
parent: 7. Delivery & Operations
nav_order: 2
---

# Discovery & Assessment

## The phase that determines everything after it

Six to ten weeks of finding out what is actually true. Skipped or rushed, and every subsequent estimate is fiction.

{: .concept }
> **Discovery's purpose is to replace assumptions with evidence, and to surface the problems that would otherwise appear in month nine.** The most valuable output is rarely the target architecture — it's the list of things nobody knew: the ungoverned population, the application with no owner, the source system whose data can't support the process everyone assumed. **Deliver bad news early; it is worth far more then than later**, and delivering it credibly is one of the fastest ways to establish yourself as an architect rather than a vendor's implementer.

---

## What to gather

### Populations

- Employees, contractors, interns, partners, customers, service accounts, machine identities.
- For each: **how many, who is authoritative, what triggers create/change/end, and how good is the data?**
- The gap you're looking for: **populations with no authoritative source.** There is almost always at least one, and it's usually contractors.

### Identity stores

Every directory, database and store holding identities. For each: what it's authoritative for, who consumes it, who writes to it, how it synchronises with the others, and how many of the same humans appear in it.

### Applications

The core inventory. For each application:

| Field | Why |
|:--|:--|
| Name, business owner, technical owner | Ownership is the foundation of governance |
| Business criticality and data classification | Drives risk tier |
| User population size and type | Scope |
| Authentication method | SSO, local, LDAP, other |
| **Provisioning capability** — SCIM, API, file, none | Determines integration effort |
| **Can you read current state?** | Determines whether reconciliation is possible |
| Entitlement model | Groups, roles, profiles, licence types |
| **Non-production environment?** | The commonly-missed blocker |
| Regulatory scope | SOX, PCI, GDPR relevance |
| Current access process | How access is granted today, by whom |

Expect the inventory to be incomplete. Cross-reference: the CMDB, SSO configuration, expense records, network/proxy logs, the service desk's ticket categories, and — reliably productive — asking each department what tools they actually use.

### Processes

Walk through them with the people who do them, not from documentation:

- Joiner, mover, leaver — including the awkward variants.
- Access request and approval.
- Certification, if it happens.
- Privileged access.
- Emergency access.
- Third-party onboarding.

For each: **who does what, how long it takes, where it breaks, what the workarounds are.** The workarounds are the most informative part; they tell you what the official process fails to do.

### Current state metrics

Baseline numbers, because you'll be measured against them:

- Leaver revocation time (median and p95), end to end.
- Orphan accounts, dormant accounts.
- Standing privileged accounts.
- Access request cycle time.
- Password reset volume and cost.
- Applications with SSO / provisioning / governance.
- Open audit findings.

{: .architect }
> **Gather the baseline numbers even if nobody asked for them, and especially if they're embarrassing.** They serve three purposes: they make the business case (11 days to revoke is a fact, not an opinion); they let you demonstrate improvement later (the *only* way to prove the programme worked); and they surface problems the organisation had normalised. Teams that have lived with an 11-day leaver process for years genuinely stop seeing it — the number makes it visible again.

---

## How to gather it

**Interviews** — IAM team, HR, application owners, service desk, infrastructure, security, audit, and a few actual end users. Twelve to twenty-five, 45 minutes each. Ask what happens when things go wrong; that's where the truth lives.

**Data extraction** — actual exports, not descriptions. Directory dumps, HR extracts, application user lists. The gap between "what people say the data looks like" and what it looks like is the single most valuable finding in most discoveries.

**Process walkthroughs** — sit with someone doing a joiner. Time it. Count the manual steps and the systems touched. This produces numbers that surprise executives.

**Technical assessment** — directory health, attack path analysis, certificate inventory, MFA coverage by method, privileged account enumeration.

**Documentation review** — policies, standards, previous audit reports, prior assessments. Previous audit findings are especially useful: they tell you what has already been promised and not delivered.

---

## Common findings

You will find most of these; recognising them in advance speeds up the work:

- **No authoritative source for non-employees.** Nearly universal.
- **More identity stores than anyone expected.**
- **Manager data 10–40% wrong** — vacant, stale, or pointing at leavers.
- **Orphan accounts in every system**, often thousands.
- **Shared accounts nobody will admit to** until you show them the login data.
- **Applications with no owner**, or an owner who left.
- **Privileged access far broader than believed** — usually 2–5× the assumed number.
- **Non-human identities entirely uncounted.**
- **Leaver processing much slower than the policy claims**, with the p95 far worse than the median.
- **An undocumented process that only one person knows**, who is retiring.
- **Applications reachable outside SSO**, bypassing every control that was assumed.

---

## The deliverables

1. **Current state assessment** — findings, evidence, quantified.
2. **Application inventory** — the artefact everyone reuses for years. Make it good.
3. **Identity data quality report** — per source, with the owning team named ([data quality](../02-identity-fundamentals/19-identity-data-quality.md)).
4. **Risk register** — specific, scenario-based ([identity risk](../06-business-and-risk/02-identity-risk.md)).
5. **Target architecture** — capabilities, not products.
6. **Gap analysis and roadmap** — sequenced increments, each independently valuable.
7. **Business case** — with the baseline numbers you just gathered.
8. **Quick wins** — three to five things deliverable in 90 days. **Always include these.**

The quick wins matter disproportionately. A discovery that produces only a two-year roadmap feels like a bill; one that also produces "here are three things we can fix by March" feels like progress, and it starts the programme with momentum instead of with a procurement.

---

## Presenting findings

- **Lead with what's working.** There always is something, and it establishes that you're assessing rather than criticising.
- **Be factual, never blaming.** "Leaver revocation takes 11 days" not "the team isn't doing leavers properly." The people in the room usually inherited the situation.
- **Quantify everything you can.**
- **Separate observation from recommendation.** People can accept a fact and disagree with a conclusion; conflating them means they reject both.
- **Pre-brief anyone who will be surprised.** Nobody should first learn of a finding about their area in a steering committee.
- **Give the bad news plainly.** Softening it means it isn't heard, and you'll be asked in month nine why nobody flagged it.

---

## Architect's checklist

- [ ] Have you identified populations with **no authoritative source**?
- [ ] Is the **application inventory** built from multiple cross-referenced sources?
- [ ] Does it record **non-production environment availability** and **read-back capability**?
- [ ] Have you looked at **actual data extracts**, not descriptions?
- [ ] Have you **timed a real joiner and a real leaver** end to end?
- [ ] Do you have **baseline metrics** for everything you'll later be measured on?
- [ ] Have you talked to the **service desk** and to real end users?
- [ ] Are findings **quantified and separated** from recommendations?
- [ ] Have you **pre-briefed** everyone who will be surprised?
- [ ] Does the output include **three to five 90-day quick wins**?

---

**Next:** [Requirements & Use Cases](03-requirements.md) →

---
title: Business Case & Stakeholders
parent: 6. Business & Risk
nav_order: 7
---

# Business Case & Stakeholders

## Building a case that survives scrutiny

### The structure

1. **The situation** — what's true today, with numbers. *"Leaver access takes a median of 11 days to revoke. We have 340 orphan accounts. We failed audit finding IA-2026-03."*
2. **The consequence of doing nothing** — the honest counterfactual, including cost trajectory. Doing nothing is always an option and must be costed like the others.
3. **The proposal** — what you'd build, in increments, in business language.
4. **The cost** — five years, all-in, including the [operating model](05-operating-model.md).
5. **The benefit** — quantified where possible, with the method shown.
6. **The risks of doing it** — delivery risk, adoption risk, dependencies. Including these makes the rest credible.
7. **The alternatives considered**, and why they were rejected.
8. **The ask** — specific: how much, for what, by when, and what you'll report back.

### Quantifying benefit honestly

| Type | Approach | Credibility |
|:--|:--|:--|
| **Hard cost saving** | Licences reclaimed, helpdesk calls avoided, contractor hours removed | High — verifiable after the fact |
| **Productivity** | Hours saved × loaded cost | Medium — discount it, since saved hours rarely become cash |
| **Risk reduction** | Scenario cost × likelihood change | Medium — depends entirely on the credibility of the scenario |
| **Compliance** | Fine avoided, finding closed, condition removed | High when there's a live finding |
| **Enablement** | Value of the thing enabled | Varies — attribute a fraction, not the whole |

{: .warning }
> **Be conservative, and show your workings.** A business case claiming €4m of benefit from softly-estimated productivity gains gets picked apart by a CFO's analyst, and when one number falls the whole case falls with it. A case claiming €900k of hard, verifiable savings plus a described risk reduction survives — and, crucially, **you can demonstrate delivery against it afterwards**, which is what earns you the next round of funding. Under-promise, show the method, and let the risk narrative carry the qualitative weight.

### The numbers you should already have

- Cost per helpdesk call, and password-reset call volume.
- Loaded hourly cost by role band.
- Hours spent per joiner, per access request, per certification cycle.
- Licence cost per user for the top ten SaaS applications, and how many leavers still hold them.
- Cost per hour of an authentication outage (ask the business, don't estimate).
- The cost of the last audit remediation.
- Peer incident costs in your sector.

Collect these once; you'll use them repeatedly.

---

## Stakeholders

### The map

| Stakeholder | Wants | Gives you | Loses if you succeed |
|:--|:--|:--|:--|
| **CISO** | Risk reduction, audit posture | Sponsorship, budget | — usually your strongest ally |
| **CIO/CTO** | Delivery, simplification | Resources, priority | Competing priorities |
| **CFO** | Cost control, ROI | Money | — |
| **Audit / risk** | Evidence, closed findings | Leverage (findings are funding) | — |
| **HR** | Their own systems working | Authoritative data — **which you need** | Extra work on data quality |
| **Application owners** | Not being disturbed | Ownership decisions | Certification effort, loss of local control |
| **Infrastructure / AD team** | Stability | Deep knowledge, access | Perceived loss of control |
| **Developers** | Speed, autonomy | Adoption | Feel constrained if you gatekeep |
| **Business unit leaders** | Their people productive | Political support | Process changes |
| **Works council / legal** (EU) | Employee rights, lawfulness | Approval — **required, not optional** | — |
| **The service desk** | Fewer calls | Ground truth about what users actually struggle with | — |
| **Vendors** | The sale | Expertise, and a case for their product | — |

### The ones people mishandle

**HR** is the most important and least cultivated relationship in enterprise IAM. They own your authoritative source; without their data quality, nothing works. They are also busy, measured on other things, and often see IAM as a source of extra work. Invest early: understand their calendar and their constraints, make your asks specific and small, and show them what their data enables. A good HR relationship is worth more than a good tool.

**Application owners** are where governance actually happens. They must accept ownership, describe entitlements meaningfully, and make certification decisions. Most don't know they're owners. Establishing that list is political work, and it's [the highest-value organisational artefact](05-operating-model.md) in the programme.

**The AD/infrastructure team** frequently experiences IGA as an outsider taking control of *their* directory. They also hold knowledge you cannot get elsewhere. Bring them in as designers rather than as recipients — the difference in outcome is dramatic.

**The service desk** knows precisely where the friction is, which access requests are most common, and what users actually complain about. Ten minutes a month with them is better ground truth than any survey.

**Works councils** (Germany, Austria, Netherlands, France and others) have genuine legal standing on employee monitoring — directly relevant to PAM session recording and behavioural analytics. Consulting them late can stop a deployment entirely, after it's built.

---

## Influence without authority

Architects rarely have authority over the teams implementing their designs. What actually works:

**Be useful before you're needed.** Help a team with a problem that isn't yours. Reciprocity is the most reliable currency there is.

**Give people the reason, not just the rule.** Engineers who understand *why* will apply a principle to a case you didn't anticipate. Engineers given only a rule will follow it literally and route around it when it's inconvenient.

**Let them shape it.** A design someone contributed to is a design they defend. Bring problems to people, not finished answers.

**Make the right thing the easy thing.** Templates, golden paths, self-service with guardrails. This beats governance boards permanently — an easy correct path outcompetes a hard correct path without any enforcement at all.

**Pick your battles, and lose some visibly.** An architect who concedes on things that don't matter is far more persuasive on the things that do.

**Publish and be consistent.** Written, dated, versioned decisions. Consistency over time is what makes people stop re-litigating.

{: .architect }
> **When you meet resistance, the objection stated is usually not the objection held.** "This won't scale" often means "I wasn't consulted." "We tried this before" often means "the last person who tried burned me." "The business won't accept it" often means "I don't want to have that conversation." The productive move is to ask questions until the real objection surfaces — because you cannot address an objection nobody has voiced, and arguing against the stated one convinces no one. This single habit resolves more architectural disagreement than any amount of technical rigour.

---

## Presenting to executives

- **Lead with the conclusion.** They may only hear the first sentence.
- **One page, three numbers, one ask.**
- **Anticipate the two questions**: *what happens if we don't?* and *what do you need?*
- **Have depth ready, but don't lead with it.**
- **Never surprise your sponsor in the room.** Pre-brief them, always.
- **Name the trade-off you're making**, so they know it's a decision and not a sales pitch.

---

## Architect's checklist

- [ ] Do you have the **standard numbers** (cost per call, loaded hourly cost, licence costs, outage cost)?
- [ ] Does the business case include the **cost of doing nothing** and the **risks of doing it**?
- [ ] Are benefits **conservative, with the method shown**?
- [ ] Does the case include the **five-year run cost**?
- [ ] Have you mapped your stakeholders, including **HR, the service desk and (in the EU) the works council**?
- [ ] Is there a maintained list of **application owners** who know they're owners?
- [ ] Have you brought the **AD/infrastructure team in as designers**?
- [ ] Is there a **paved road** that makes the right thing easier than the alternative?
- [ ] Have you **pre-briefed your sponsor** before every executive presentation?
- [ ] When you last met resistance, did you find the **real objection**?

---

**Next:** [Stage 7 — Delivery & Operations](../07-delivery/) →

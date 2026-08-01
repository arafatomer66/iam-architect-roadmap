---
title: Business Alignment
parent: 6. Business & Risk
nav_order: 1
---

# Business Alignment

## What "aligned" actually means

Not "the business approves of what we're doing." It means **the IAM roadmap is derived from business objectives**, and each item can be traced to one.

If the company's objectives this year are *expand into two new markets*, *complete the acquisition*, *reduce operating cost by 8%* and *achieve DORA compliance*, then your roadmap should visibly serve those:

| Business objective | Identity contribution |
|:--|:--|
| Expand into two markets | Identity that supports new legal entities, local data residency and local regulatory requirements — without a six-month project each time |
| Complete the acquisition | Day-one collaboration, day-30 joint access processes, year-one consolidation ([migration](../04-architecture-practice/04-migration-and-coexistence.md)) |
| Reduce operating cost 8% | Automated JML (removing manual provisioning effort), self-service password reset, reclaiming licences from leavers and dormant accounts |
| DORA compliance | Access control evidence, third-party access governance, incident response for identity |

{: .architect }
> **Ask for the strategy document, and read it.** Most IAM architects have never read their own organisation's strategic objectives, then wonder why funding is hard. The roadmap that says "here is how identity serves each of the board's four priorities" is in a completely different conversation from the one that says "here is what we'd like to build". This takes an afternoon and changes how your work is received for years.

---

## The four value narratives

Almost every identity investment fits one of these. Know which one you're telling, and don't mix them in a single slide.

### 1. Risk reduction

*"This reduces the likelihood or impact of an incident."*

Strongest when tied to a specific, credible scenario and to something that has actually happened — to you, or to a peer in your sector. Weakest when it's generic ("improves security posture").

**Make it concrete:** "Credential-based attacks caused 3 of our last 4 security incidents. Phishing-resistant MFA for the 400 people with privileged access removes that vector for the population that matters most."

### 2. Compliance

*"This is required, or closes a finding."*

The easiest to fund and the most limited: it gets you exactly what the requirement demands and no more. Use it, but don't let it become the *only* narrative — a compliance-only programme delivers a compliant organisation that is not necessarily a secure one, and it stops the moment the finding closes.

### 3. Efficiency

*"This costs less than what we do today."*

The most persuasive to finance, and it requires real numbers:

| Source | Typical calculation |
|:--|:--|
| Password reset calls | Volume × cost per call (commonly €15–30 fully loaded) |
| Manual provisioning | Hours per joiner × joiners per year × loaded hourly cost |
| Access request handling | Requests per year × handling time × cost |
| Certification effort | Reviewers × items × minutes per item |
| Licence reclamation | Dormant/leaver accounts × licence cost. **Frequently the single largest and easiest-to-verify number** |
| Audit preparation | Person-weeks per cycle spent assembling evidence manually |

Use your organisation's own numbers, and be conservative. A business case that survives scrutiny beats one that impresses and then gets picked apart.

### 4. Enablement

*"This makes something possible that isn't today."*

The most strategic and the hardest to quantify: a new customer-facing product needing CIAM, a partner ecosystem needing B2B federation, an M&A capability, a cloud migration that can't proceed without workload identity.

Quantify it by the value of the thing being enabled, not by the identity work itself.

---

## Speaking to each audience

| Audience | Cares about | Opening line that works | Never |
|:--|:--|:--|:--|
| **Board / CEO** | Existential risk, reputation, strategic enablement | "Identity is how 90% of breaches start. Here's our exposure and what closing it costs." | Protocol names |
| **CFO** | Cost, ROI, predictability | "This saves €X annually and avoids €Y of licence spend on leavers." | Anything unquantified |
| **CIO / CTO** | Delivery, complexity, technical debt | "This reduces the number of identity systems from six to two and cuts application onboarding from three weeks to two days." | Vendor enthusiasm |
| **CISO** | Risk, controls, audit posture | "This closes findings 3 and 7 and reduces privileged standing access by 80%." | Over-promising coverage |
| **Business unit leader** | Their team's productivity | "Your new starters will be working on day one, and access requests will resolve in hours." | Governance vocabulary |
| **Auditor** | Evidence, control design and operation | "Here's the control, how it operates, and the evidence for the period." | Defensiveness |
| **Engineers** | Clarity, autonomy, not being blocked | "Here's the paved road; it's faster than building your own." | Gatekeeping |

{: .warning }
> **The most common self-inflicted wound is technical vocabulary in a business conversation.** Saying "we'll federate via SAML with SCIM provisioning" to a CFO signals that you can't distinguish between what you do and why it matters — and the funding goes to someone who can. Practise the reverse: **explain the design without naming a single protocol.** If you can't, you don't yet understand its value.

---

## Getting a seat at the table

Practical, and they compound:

- **Attend business planning cycles.** Identity constraints discovered in the planning meeting are cheap; discovered in delivery, expensive.
- **Publish something regularly** — a short monthly note on identity risk and progress, in business language. Consistency builds recognition, and recognition is what gets you invited.
- **Be the person who says yes with conditions**, not the person who says no. "We can do that; here's the fast path and here's what it costs" beats "that's not compliant with our standards" every time.
- **Fix something visible early.** Password reset self-service, or day-one readiness for new hires. Credibility earned on something people feel funds the invisible work later.
- **Learn the business.** How does the company make money? What is month-end close, and why does it make finance unavailable? Which regulator matters most? Architects who understand this design differently, and better.

---

## When the business is wrong

It happens: an executive wants an exception that creates real risk, or a project wants to ship without governance.

The approach that works:

1. **State the risk factually**, in their terms. Not "that's insecure" but "this means a leaver would retain access to customer data for up to 30 days, and we'd have no record of who accessed what."
2. **Offer alternatives**, including a faster-but-controlled path.
3. **If overruled, document it** — a risk acceptance with a named owner, a review date and an expiry. Not to say *I told you so*, but because a risk with an owner and a date is genuinely different from an unowned one: it gets revisited.
4. **Escalate only what's genuinely serious.** An architect who escalates everything is routed around within a quarter. Spend that capital carefully.

{: .architect }
> **A documented, time-boxed, owned risk acceptance is a legitimate architectural outcome, not a failure.** The organisation is entitled to accept risk; your job is to make sure it's doing so *knowingly*, with the right person's name on it and a date to revisit. Architects who treat every accepted risk as a personal defeat burn out and lose influence. Architects who make risk visible, owned and time-bound become the person executives consult before deciding.

---

## Architect's checklist

- [ ] Have you **read your organisation's strategic objectives** this year?
- [ ] Can every roadmap item be traced to one of them, or to a named risk or finding?
- [ ] For each initiative, which of the **four narratives** are you telling — and is it the right one for the audience?
- [ ] Do you have **real numbers** for the efficiency case, from your own organisation?
- [ ] Can you explain your architecture **without naming a protocol**?
- [ ] Are you in the room during **business planning**, or hearing about projects afterwards?
- [ ] Is there a **visible early win** that bought you credibility?
- [ ] Are accepted risks **documented, owned and time-boxed**?

---

**Next:** [Identity Risk](02-identity-risk.md) →

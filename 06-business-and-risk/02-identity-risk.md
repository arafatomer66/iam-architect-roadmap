---
title: Identity Risk
parent: 6. Business & Risk
nav_order: 2
---

# Identity Risk

## Why identity risk is treated badly

Most organisations record identity risk as one line — *"inappropriate access"* — rated Medium, owned by IT, unchanged for four years. That line does no work: it prompts no decision, funds nothing, and expires without anyone noticing.

Useful risk statements are **specific, scenario-based, quantified where possible, and owned by someone who can act.**

{: .concept }
> **A risk is a statement about a possible future with a cause, a consequence and a likelihood.** "Weak access controls" is not a risk; it's a condition. "**A departing employee retains access to the customer database for up to 30 days because leaver processing is manual and averages 11 days, which could result in data exfiltration affecting up to 2.1 million records**" is a risk — it names the cause, the consequence, the scale and, implicitly, the fix.

---

## The identity risk register

Risks that belong on it, with the questions that make each concrete:

| Risk | Make it concrete by asking |
|:--|:--|
| **Leaver access retention** | What's the median and p95 revocation time, end to end? How many leavers per year? What can they reach? |
| **Privilege accumulation** | How many people hold access from a role they left over a year ago? |
| **Standing privileged access** | How many accounts hold permanent administrative rights? What's the blast radius of one? |
| **Credential compromise** | What proportion of privileged users have phishing-resistant MFA? Where is MFA absent entirely? |
| **Orphan and shared accounts** | How many accounts have no owner? How many people share one credential? |
| **Non-human identity sprawl** | How many credentials exist with no owner, no expiry and no rotation? |
| **IdP compromise** | What's the blast radius? Could you detect it after the fact? |
| **Third-party access** | How many external identities have privileged access? How would you know if one left their employer? |
| **SoD violations** | How many toxic combinations exist? How many are "mitigated" without evidence the mitigation operates? |
| **Audit failure** | Which findings are open? What's the regulator's tolerance? |
| **Availability** | What is the cost per hour of an authentication outage? |
| **Data quality** | How many identities can't be processed automatically, and what does that block? |

**Every one of those questions is answerable with data you either have or should have.** Producing the numbers is itself the highest-value early work in a programme — it converts a vague concern into a fundable proposition.

---

## Quantifying credibly

You will be asked "how much risk?" Three levels of rigour, in increasing order:

**1. Ordinal (likelihood × impact).** The standard heat map. Fast, universally understood, and it cannot compare two Mediums or justify a budget. Fine for triage, weak for decisions.

**2. Scenario-based.** Describe a specific, plausible chain and estimate its cost:

> *A contractor's account remains active 30 days after their engagement ends. They (or someone who obtains the credential) access the customer database and exfiltrate 200,000 records. Costs: regulatory fine, notification, credit monitoring, legal, incident response, customer churn, and management time. Comparable incidents in our sector have cost €4–12m.*

This is usually the sweet spot: rigorous enough to be defensible, simple enough to be understood in a meeting.

**3. Quantitative (FAIR-style).** Decompose into loss event frequency and loss magnitude, use ranges rather than point estimates, and simulate. Produces a loss-exceedance curve: *"90% confidence that annual loss from identity-related incidents is between €0.4m and €6m."*

{: .architect }
> **You do not need to become a quantitative risk analyst — you need to stop offering unquantified assertions.** The move that changes conversations is going from "this is a high risk" to "**here is the scenario, here is what comparable incidents cost, here is our exposure, and here is what the control costs.**" The moment a risk has a number next to a control cost, it becomes a decision a business person can make — and business people fund decisions, not concerns.

---

## Risk in the design, not just the register

Risk thinking belongs inside architecture:

**Blast radius per component.** For every element: if this is compromised, what is reachable? This is what justifies tiering, segmentation and separation of duties inside IAM.

**Failure mode per component.** Fails open or closed? Both are wrong in some contexts; the error is not choosing ([architectural thinking](../04-architecture-practice/01-architectural-thinking.md)).

**Attack paths, not just controls.** Attackers chain weaknesses. A low-privilege account plus an ACL misconfiguration plus an unconstrained delegation equals domain admin. Path analysis reveals risk that component-by-component review misses entirely — this is what [ITDR](../08-frontier/02-itdr.md) tooling automates.

**Aggregation.** Twenty applications each with "acceptable" risk, all reachable with one credential, is not twenty small risks — it's one large one. Identity is precisely the layer where aggregation happens, which is why identity risk is systemic rather than local.

---

## Communicating risk

**Use a scenario, not a control gap.** "We don't do access certification" invites "so what?". "We cannot demonstrate to the regulator who had access to trading systems last quarter, and the last firm in our position was fined €X" does not.

**Show the trend, not the snapshot.** "Standing privileged accounts: 340 → 180 → 95 over three quarters" tells a story of a programme working. A single number tells nothing.

**Be honest about what you don't know.** "We have no visibility of non-human identities in the cloud estate" is a finding worth reporting. Unknown exposure is a risk in itself, and reporting it is how you get funded to close it.

**Never inflate.** Credibility is your entire currency. An architect caught overstating a risk to win a budget argument is discounted permanently — including on the occasion when the risk is real.

---

## Risk acceptance

Not every risk gets fixed. A proper acceptance has:

- **A named individual** — senior enough to own the consequence, and not you.
- **The scenario and the estimated impact**, in writing.
- **Why it isn't being remediated** — cost, timing, business need.
- **Compensating controls**, if any, and **evidence they operate**.
- **An expiry date and a review.**

{: .warning }
> **Accepted risks with no expiry are how organisations accumulate invisible exposure.** A risk accepted in 2021 for a system that was "being decommissioned next year" is still accepted, the system is still running, and the accepting executive left in 2023. Every acceptance needs a date, and the register needs a periodic sweep for accepted risks whose owner has departed or whose justification has expired. That sweep is a five-minute report that regularly surfaces genuine surprises.

---

## Architect's checklist

- [ ] Are identity risks in the register **specific and scenario-based**, or one vague line?
- [ ] Can you answer the concrete question behind each risk **with data**?
- [ ] Is at least the top-tier risk **quantified** in monetary terms?
- [ ] Do you report **trends**, not snapshots?
- [ ] Is **blast radius** analysed per component in your designs?
- [ ] Are **attack paths** analysed, not just individual controls?
- [ ] Is **risk aggregation** across applications accounted for?
- [ ] Do accepted risks have an **owner, evidence of compensating controls, and an expiry**?
- [ ] Is there a periodic sweep for **accepted risks whose owner has left**?
- [ ] Have you reported anything you **don't have visibility of**?

---

**Next:** [Compliance & Regulation](03-compliance.md) →

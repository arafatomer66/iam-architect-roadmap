---
title: Metrics, KPIs & Reporting
parent: 6. Business & Risk
nav_order: 6
---

# Metrics, KPIs & Reporting

## Measure what changes decisions

Most IAM dashboards report what's easy to count: number of accounts, number of requests, number of applications. None of those tell anyone whether the estate is getting safer.

{: .concept }
> **A good metric changes a decision.** Before adding one, ask: *what would we do differently if this number went up or down?* If the answer is "nothing", it's a statistic, not a metric — put it in an appendix. If the answer is "we'd reprioritise, fund something, or escalate", it belongs on the dashboard. Most IAM reporting can be cut by two-thirds using this test alone, and it becomes far more persuasive as a result.

---

## The metrics worth having

### Risk reduction

| Metric | Why it changes decisions |
|:--|:--|
| **Leaver revocation time** — median and p95, end to end from the real-world event | The audit metric; p95 exposes the tail where the real risk is |
| **Standing privileged accounts** | The blast-radius number; drives PAM/JIT investment |
| **% of privileged users on phishing-resistant MFA** | Directly maps to the dominant attack vector |
| **Orphan accounts** — count and age | Data integrity and a real exposure |
| **Dormant accounts** (no auth in 90 days) | Free risk reduction; also free licence recovery |
| **Open SoD violations**, and "mitigated" ones without evidence | Compliance exposure |
| **NHIs with no owner / no expiry / not rotated** | The largest ungoverned population |
| **Accounts not covered by governance**, weighted by system risk | Honest coverage |

### Coverage

| Metric | Note |
|:--|:--|
| % of applications with SSO | Easy, and often the only one reported |
| % with automated provisioning | Harder, more valuable |
| % with reconciliation | **The one that matters most and is reported least** |
| % under certification | Should be risk-scoped, not universal |
| **Coverage weighted by risk, not by count** | 90% of applications can be 50% of risk. Always report both |

### Efficiency

| Metric | Use |
|:--|:--|
| Access request cycle time (median, p95) | Whether people will use the process or route around it |
| % of requests fulfilled without human touch | Automation quality |
| Provisioning failure rate | Drives the exception-queue headcount |
| Password reset self-service deflection | Direct cost saving |
| Application onboarding lead time | Whether IAM is a bottleneck |
| Licences reclaimed from leavers/dormant accounts | Hard cash, easily verified |

### Governance health

| Metric | Note |
|:--|:--|
| Certification completion rate | Necessary, not sufficient |
| **Revocation execution rate** | The metric that proves the control operated. Report both together |
| % of decisions that were "revoke" | Suspiciously low (<2%) suggests rubber-stamping |
| Role coverage; single-member roles | Model health ([role modelling](../02-identity-fundamentals/16-role-modelling.md)) |
| Entitlements with no owner / no description | Catalogue quality |
| Exception count and **age** | Whether exceptions expire or accumulate |

### Operational

Availability of AM and IGA, authentication success rate and latency, sync lag, exception queue depth and age, incident count and MTTR.

---

## Reporting by audience

The same underlying data, three very different products:

**Board / executive — one page, quarterly.** Three to five numbers with trend arrows, one sentence each. Risk posture, one headline achievement, one ask. **No tables.** If you cannot fit it on one page, you have not decided what matters.

**CISO / IT leadership — monthly.** Ten to fifteen metrics with trends, exceptions and their ages, programme progress against plan, risks needing decisions, and what's blocked and by whom.

**Operational team — weekly or daily.** Queue depths, failure rates, SLA attainment, upcoming campaign deadlines, certificate expiries, sync health.

**Audit — per cycle.** Control operation evidence, population completeness, sample results, exception handling.

**Application owners — per campaign.** Their applications, their users, their outstanding decisions. Nothing else — anything more generates ignored email.

{: .architect }
> **The executive report should tell one story, not present a dashboard.** "Leaver revocation went from 11 days to 4 hours; the residual risk is now concentrated in 12 legacy applications; closing those needs €X and two quarters." That's a narrative with an ask. Twenty metrics with no narrative produces polite nodding and no decision — and then you wonder why the programme wasn't funded.

---

## Trends beat snapshots

A single number has almost no information. "340 orphan accounts" — is that good? Compared with what?

Always show direction, and be honest when it's wrong. A metric that gets worse and is reported with an explanation builds credibility. One that quietly disappears from the deck destroys it — and people notice.

Set **targets with dates** and report against them. "Standing privileged accounts: 340 → 95, target 40 by Q4" is a programme. "Standing privileged accounts: 95" is trivia.

---

## Metrics that mislead

| Metric | Why it misleads |
|:--|:--|
| **Number of applications integrated** | Says nothing about risk covered. Weight by risk |
| **Certification completion %** | 100% completion with 0% revocations means rubber-stamping, not control |
| **Number of accounts** | Grows with the business; means nothing on its own |
| **MFA coverage %** | Hides which method. 95% on SMS is weaker than 60% on passkeys |
| **Tickets closed** | Optimising this rewards a team for a process that generates tickets |
| **Maturity model score** | Self-assessed, unfalsifiable, and moves when the assessor changes |
| **Time from *system receiving* an event** | Hides upstream delay. **Measure from the real-world event** |

That last one deserves emphasis: an IGA platform reporting "leaver processed in 4 minutes" while HR recorded the termination nine days after the person left is reporting a number that is true and useless. **End-to-end or don't bother.**

---

## Building the reporting

- **Automate it.** Manually assembled reports are late, inconsistent and quietly abandoned. If a metric can't be generated automatically, that's a finding about your platform.
- **One source of truth**, so numbers don't differ between decks.
- **Define every metric precisely**, in writing — what counts as a "leaver", what counts as "privileged". Undefined metrics get argued about instead of acted on.
- **Version the definitions**, so a change in methodology doesn't look like a change in performance.
- **Publish the raw data** to those who want it, so nobody suspects curation.

---

## Architect's checklist

- [ ] Does every reported metric pass the **"what decision does this change?"** test?
- [ ] Is leaver revocation measured **end to end from the real-world event**, with p95?
- [ ] Is coverage reported **weighted by risk** as well as by count?
- [ ] Are certification completion **and revocation execution** reported together?
- [ ] Is MFA coverage broken down **by method strength**?
- [ ] Do all metrics show **trends against dated targets**?
- [ ] Is the executive report **one page with a narrative and an ask**?
- [ ] Is reporting **automated from a single source**, with versioned definitions?
- [ ] Are metrics that got **worse** still in the deck?

---

**Next:** [Business Case & Stakeholders](07-stakeholders.md) →

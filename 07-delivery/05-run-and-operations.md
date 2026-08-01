---
title: Run & Operations
parent: 7. Delivery & Operations
nav_order: 5
---

# Run & Operations

## The phase that lasts

Implementation takes a year or two. Run lasts a decade. Yet run is designed last, funded least and documented worst — which is why platforms that launched successfully are so often unrecognisable three years later.

{: .concept }
> **Operability is an architectural quality attribute, not an operations problem.** A design that requires a specialist to diagnose every failure, that surfaces errors only in a log file, or that has no way to re-drive a failed operation, has made a permanent staffing decision on behalf of the organisation. **Ask "how will someone at 3am know what's wrong and what to do about it?" while you are still designing** — it is far cheaper to answer then.

---

## The daily reality

What an IAM operations team actually does, most days:

| Activity | Frequency |
|:--|:--|
| Clear the **exception queue** — failed provisioning, uncorrelated accounts, quarantined records | Daily, and it is the job |
| Handle access requests that need human fulfilment | Daily |
| Investigate "I can't access X" | Daily |
| Check sync and feed health | Daily |
| Onboard applications | Continuous |
| Chase certification non-responders | Cyclical, intensely |
| Answer audit questions | Cyclical |
| Manage certificate and credential expiry | Monthly |
| Apply and test upgrades | Quarterly, or on the vendor's schedule |

**The exception queue is the health indicator for the whole platform.** Depth and age tell you more than any dashboard: a queue that is cleared daily means the platform works; a queue with items three weeks old means access is wrong somewhere and nobody knows where.

---

## Runbooks

Written for someone competent but unfamiliar, at 3am, under pressure. That constraint shapes everything.

**The ones you must have:**

| Runbook | Trigger |
|:--|:--|
| **Emergency access / break-glass** | Identity systems unavailable |
| **Terminate all sessions for a user** | Compromise or urgent leaver |
| **Emergency leaver** | For-cause termination |
| **Provisioning failure re-drive** | Failed operations in the queue |
| **HR feed didn't arrive** | Absence alert |
| **Certificate expiry / emergency rotation** | Alert, or an outage |
| **Sync failure / large delta refused** | Deletion threshold triggered |
| **IdP failover** | Regional or component failure |
| **Restore from backup** | Data loss |
| **Bulk change rollback** | An erroneous mass operation |

**What makes a runbook usable:**

- Starts with **how to confirm this is the right runbook** (symptoms, checks).
- Explicit commands and screens, not prose descriptions.
- **Decision points made explicit**, including when to escalate and to whom, by name and number.
- Prerequisites listed up front — access needed, approvals required.
- A verification step: how do you know it worked?
- **Stored where it can be read when systems are down.** A runbook in a wiki behind SSO is unavailable during exactly the incident it was written for. Keep an offline copy.
- Dated, owned, and **tested** — an untested runbook is a hypothesis.

---

## Monitoring

Three layers, and most organisations build only the first:

**1. Infrastructure** — CPU, memory, disk, database, queue depth. Necessary, insufficient.

**2. Service** — authentication success rate and latency, token issuance, provisioning success rate, sync lag, API error rates.

**3. Business outcome** — the layer that catches what the others miss:

- Joiners not provisioned by their start date.
- Leavers not revoked within SLA.
- Certification campaign progress against deadline.
- Exception queue depth and **age**.
- Orphan account count trending.
- Feeds not received.
- Certificates expiring in 90/30/7 days.

{: .architect }
> **Synthetic transactions are the highest-value monitoring you can build for an identity estate.** A robot that performs a real login every minute — from multiple network locations, through the real flow — detects what infrastructure metrics never will: the login page returning 200 while authentication is broken, the federation to one SP failing while others work, latency degradation before users complain. Extend it to a synthetic provisioning transaction (create a test identity, verify it appears in three targets, delete it) and you have end-to-end assurance that no component-level metric provides.

---

## Alert discipline

Alerts that don't produce action train people to ignore alerts — including the real ones.

Rules: every alert has a **runbook**; every alert has a **recipient who can act**; alerts are **reviewed monthly** and the noisy ones are fixed or deleted; **paging is reserved for things that genuinely need someone now**; everything else goes to a queue that is worked in hours.

The test: if an alert fired and nobody did anything, either the alert or the process is wrong. Fix one of them.

---

## Change management for identity

Identity changes carry unusual risk, because the failure mode is "nobody can work."

- **Staged rollout** for policy changes: a pilot group, then a business unit, then everyone. Never estate-wide in one step.
- **Simulation first** — every major platform offers what-if or preview for policy and rule changes. Use it every time.
- **Snapshot before bulk operations**, diff afterwards ([testing](04-testing.md)).
- **A rollback plan** for every change, tested where feasible.
- **Change freezes** during business-critical periods — month-end close, peak trading, year-end.
- **Four eyes** on anything affecting authentication or authorisation policy.
- **Emergency change process** that is fast and still leaves a record.

---

## Continuous improvement

Run is not steady state. Every quarter, look at:

- **Top ticket categories** — what is generating the most work, and can it be automated away?
- **Exception patterns** — are the same connectors failing repeatedly?
- **Access request patterns** — frequently requested items should become birthright or role-based.
- **Certification revocation patterns** — entitlements always revoked should be reviewed at source.
- **Dormant access** — remove it automatically rather than asking about it.
- **Aged exceptions and risk acceptances** — sweep for expired ones and ones whose owner has left.

The goal is a platform where the operational load per identity falls over time. If it rises with headcount, something in the design is generating work — and finding it is architecture work, not operations work.

---

## Knowledge and continuity

- **Document decisions, not just procedures.** The [ADR trail](../04-architecture-practice/08-documentation-and-adrs.md) is what stops a successor undoing something important.
- **Cross-train.** A platform only one person can operate is a resignation away from a crisis — and that person cannot take a holiday.
- **Rotate the on-call and the exception queue**, so knowledge spreads.
- **Keep a decision log for operational exceptions too**, so "why is this account excluded from the policy?" has an answer.
- **Review vendor release notes** as a scheduled activity. SaaS changes arrive whether or not you read them.

---

## Architect's checklist

- [ ] Is **operability** treated as a designed quality attribute?
- [ ] Does the **exception queue** have a named owner and a daily SLA? What is its current age?
- [ ] Do the ten essential **runbooks** exist, and have they been tested?
- [ ] Are runbooks **accessible when the systems are down**?
- [ ] Is there **business-outcome monitoring**, not just infrastructure and service metrics?
- [ ] Are there **synthetic login and provisioning transactions**?
- [ ] Does every alert have a runbook and an actionable recipient?
- [ ] Are policy changes **simulated and staged**, never estate-wide at once?
- [ ] Is **snapshot-and-diff** standard for bulk operations?
- [ ] Is operational load per identity **falling over time**?
- [ ] Can the platform be operated if **any one person** is unavailable?

---

**Next:** [Identity Incident Response](06-incident-response.md) →

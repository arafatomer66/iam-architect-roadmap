---
title: SoD & Access Certification
parent: 2. Identity Fundamentals
nav_order: 17
---

# Segregation of Duties & Access Certification

## Segregation of Duties

### The principle

No single person should be able to execute a complete high-risk transaction end to end, because that enables fraud or error to go undetected.

The canonical examples:

| Toxic combination | The risk |
|:--|:--|
| Create vendor **+** approve payment | Invent a supplier and pay yourself |
| Create purchase order **+** receive goods **+** approve invoice | Fictitious purchases |
| Develop code **+** deploy to production | Unreviewed change into production |
| Administer a system **+** review its audit logs | Erase your own tracks |
| Grant access **+** approve access requests | Grant yourself anything |
| Process payroll **+** maintain employee master data | Ghost employees |
| Trade **+** confirm/settle trades | Unauthorised trading, hidden losses |

{: .concept }
> **SoD is a business control expressed in access terms, not a technical rule.** The conflict exists in the *business process*; access is merely the mechanism through which it becomes possible. This is why an SoD ruleset must be authored with finance, risk and process owners — an IAM team writing SoD rules alone will produce technically coherent rules that don't match how the business actually commits fraud.

### Preventive vs detective

| Type | When | Trade-off |
|:--|:--|:--|
| **Preventive** | Check at request/approval time; block or require risk acceptance | Far cheaper. Avoids ever having to remove something someone is using |
| **Detective** | Scan existing assignments; report violations | Necessary for pre-existing access, direct changes, and combinations created by role edits |

You need both. Preventive stops new violations; detective catches what arrived some other way — a role's contents changed, an administrator granted access directly, or a merger imported a whole population.

### Mitigating controls

Sometimes a violation cannot be resolved. A five-person finance team in a subsidiary genuinely cannot separate all duties. The answer is not to pretend the rule doesn't apply, but to record a **mitigating control**:

- An explicit, named owner who accepts the risk.
- A compensating control (dual review of all transactions above a threshold, monthly log review by an independent party, transaction-level monitoring).
- An **expiry date** and a re-review cadence.
- Evidence the compensating control actually operates — an unreviewed mitigating control is worse than an open violation, because it looks resolved.

{: .warning }
> **Mitigating controls are where SoD programmes quietly die.** The pattern: violations are found, they're too disruptive to fix, everything gets a mitigating control, nobody checks whether the compensating controls run, and two years later 400 "mitigated" violations sit in a spreadsheet nobody has opened. If you record a mitigation, you must also record who performs the compensating control, how often, and where the evidence lives — and report on it.

### Design considerations

- **Rules are combinations of *functions*, not of entitlements.** "Create vendor" may be granted by five different entitlements across three systems. Map functions → entitlements, and maintain that mapping as systems change.
- **Cross-application SoD is the hard part.** Within SAP, GRC tools handle it well. Create-vendor in SAP + approve-payment in a treasury system is where most tools struggle and most real risk lives.
- **Simulate before you enforce.** Turning on a new ruleset in a live estate typically surfaces hundreds of violations. Run it in report mode first, triage, then enforce.
- **Re-evaluate on role change**, not just on assignment. See [role modelling](16-role-modelling.md).

---

## Access certification

### What it's for

Periodic confirmation by an accountable human that access is still appropriate. It's a required control under SOX, ISO 27001, PCI DSS, NIST 800-53 and most sector regulations — and it's the control most likely to be performed as theatre.

### Campaign types

| Type | Reviewer | Good for |
|:--|:--|:--|
| **Manager review** | Line manager | Broad coverage; weak on technical entitlements the manager doesn't understand |
| **Application/data owner review** | Owner of the system | Meaningful decisions about what an entitlement actually permits |
| **Role membership review** | Role owner | Keeping role assignment honest |
| **Role content review** | Role owner + app owner | Keeping the role model honest — frequently skipped |
| **Privileged access review** | Security + owner | Highest value per item reviewed |
| **Micro-certification** | Whoever is relevant | Event-triggered: transfer, anomaly, new high-risk grant |
| **Orphan/service account review** | Claimed owner | Bringing [NHIs](../03-identity-domains/04-non-human-identities.md) under control |

### Why campaigns fail

**Volume.** A manager with 30 reports, each with 40 entitlements, faces 1,200 decisions. They will click approve-all. Everything else on this list follows from that one number.

**Unintelligible items.** `CN=APP_FIN_SAP_ZFI0042_RW_PRD` tells the reviewer nothing. If they can't tell what it does, "approve" is the only rational choice.

**Wrong reviewer.** Managers know their people; they don't know what a database role grants.

**No consequence.** If nobody chases non-responders and nothing happens when the deadline passes, the campaign is optional — and everyone learns that.

**Revocations that don't execute.** The worst outcome of all: decisions are made, evidence is produced, and the revocations sit unfulfilled. You now have documented proof that you knew about inappropriate access and left it in place.

### Designing a campaign people actually complete

1. **Scope by risk, not by completeness.** Certifying everything annually is worse than certifying the top 20% quarterly and ignoring genuinely low-risk access. Auditors accept risk-based scoping when it's documented and justified.
2. **Translate entitlements into business language.** This is catalogue work, and it pays back everywhere.
3. **Pre-filter the obvious.** Don't ask about access that everyone in the population has by birthright — certify the birthright *rule* once instead of the 5,000 resulting grants.
4. **Show context.** Last used date, peer comparison ("3 of 40 peers have this"), risk rating, when it was granted and by whom. Reviewers make better decisions with three seconds of context.
5. **Make revoke the easy path** for anything unused. Default toward removal where the evidence supports it.
6. **Escalate.** Non-response escalates to the reviewer's manager, then to a named executive.
7. **Close the loop and measure it.** Revocation completion rate is the metric that proves the control operated. Report it.

{: .architect }
> **Certification is a detective control with a very poor cost/benefit ratio compared to the alternatives.** It is expensive (thousands of hours of manager time), slow (quarterly at best) and unreliable (rubber-stamping). It exists largely because regulators require it. So the architect's move is to **make it smaller**: time-bound access removes the need to certify what will expire anyway; automated mover reviews catch privilege creep at the moment it happens; usage analytics remove dormant access without asking anyone; birthright rules get certified once instead of per user. Then certify what remains — a much smaller, higher-value set — properly.

---

## Evidence

Whatever the campaign design, the auditable artefact must show:

- **What** was reviewed (identity, entitlement, system, at what point in time).
- **Who** reviewed it, and that they were the appropriate reviewer.
- **When**, and within the required period.
- **What decision** was made, with any comments.
- **What happened next** — for revocations, proof of execution with a timestamp.
- **Exceptions**: who didn't respond, what was escalated, what remains open.

If your platform can't produce that as a report, you will produce it manually every cycle, forever. Test the evidence output during selection, not after go-live.

{: .vendor }
> **In the products.** **SailPoint** is generally strongest here — flexible campaign design, useful reviewer context and recommendations, mature evidence reporting. **One Identity Manager** provides attestation policies and procedures with fine-grained control over what is attested and by whom, integrated with its `Person`/role model. **Saviynt** offers campaign templates plus analytics-driven recommendations. For SAP-heavy estates, **SAP GRC Access Control** is often already in place for in-SAP SoD, which raises a common architecture question: who owns cross-application SoD, GRC or IGA? Decide it explicitly — the usual answer is IGA for cross-application, GRC for intra-SAP, with a defined integration and no duplicated rulesets.

---

## Architect's checklist

- [ ] Does an **SoD ruleset** exist, authored with the business, mapping functions to entitlements?
- [ ] Are SoD checks **preventive** at request time, as well as detective?
- [ ] Are violations re-evaluated when a **role's contents** change?
- [ ] Do mitigating controls have an **owner, an expiry, and evidence** that the compensating control operates?
- [ ] Is cross-application SoD covered, and is ownership between GRC and IGA explicit?
- [ ] Are certification campaigns **scoped by risk**, with a documented rationale?
- [ ] Do reviewers see **business-language descriptions, last-used data and peer context**?
- [ ] What is the **revocation completion rate**, and who owns it?
- [ ] What happens to **non-responders** — is there real escalation?
- [ ] Can the platform produce complete **audit evidence** without manual assembly?
- [ ] What have you done to make certification **smaller** — expiry, mover reviews, usage-based removal?

---

**Next:** [Privileged Access Management](18-privileged-access-management.md) →

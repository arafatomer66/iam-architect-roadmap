---
title: Requirements & Use Cases
parent: 7. Delivery & Operations
nav_order: 3
---

# Requirements & Use Cases

## Requirements that do work

A requirement earns its place if it can be **tested**, if it **discriminates** between options, and if someone **owns** it. Most requirement documents fail all three: hundreds of statements, equally weighted, mostly untestable, copied from a vendor's datasheet.

{: .concept }
> **Write requirements as use cases with acceptance criteria, not as feature statements.** "Must support SoD" is unverifiable — every vendor says yes. "*When a user requests an entitlement that conflicts with one they already hold, the system must block the request before approval, name the conflicting entitlement, and offer a documented risk-acceptance path routed to the risk owner*" is testable, discriminating, and reveals real differences between products in ten minutes of a PoC.

---

## Structure

### Functional requirements as use cases

```
UC-014 · Internal transfer with access review

Actor:        Line manager (receiving)
Trigger:      HR records a department change for an active employee
Precondition: Identity is active; new department has a defined manager

Flow:
  1. Within 1 hour of the HR event, the system evaluates birthright access
     for the new department and provisions any additions.
  2. The system identifies entitlements derived from the previous role.
  3. A targeted review listing those entitlements is raised to the
     receiving manager, with a 14-day deadline.
  4. Entitlements confirmed as needed are retained with a recorded reason.
  5. Entitlements not confirmed within 14 days are automatically revoked.
  6. Evidence of the review, decisions and revocations is retained.

Alternate flows:
  4a. Manager delegates the review → delegate decisions, both recorded.
  5a. Manager requests an extension → one extension of 7 days, logged.
  6a. Employee holds a dual role during transition → time-boxed retention
      with an explicit end date.

Acceptance criteria:
  ✓ New access provisioned within 1 hour of the HR event
  ✓ Previous-role entitlements identified accurately (tested against
    5 sample transfers from last year's real data)
  ✓ Unconfirmed access revoked automatically at day 14
  ✓ Full evidence exportable for an audit sample
```

That single use case tests a dozen product capabilities at once — and it is written entirely in terms of *your* process, so no vendor can answer it from a datasheet.

### Non-functional requirements with numbers

| Category | Requirement | Verification |
|:--|:--|:--|
| Performance | 500 authentications/sec sustained, p95 < 300 ms | Load test |
| Availability | AM 99.95% monthly; IGA 99.5% | Design review + SLA |
| Scale | 50,000 identities, 200 applications, 3M entitlement assignments | Volume test |
| Bulk | 3,000 lifecycle events in 4 hours without breaching target rate limits | Bulk test |
| Revocation latency | Privileged access revoked within 15 min of the HR event, end to end | Timed test |
| Recoverability | RPO 15 min, RTO 4 hours | DR test |
| Evidence | Certification evidence exportable without manual assembly | Demonstrate |
| Operability | Failed operations visible in one queue with re-drive capability | Demonstrate |

**Every number needs an owner** — the person who agreed it and will be affected if it's missed.

---

## The use cases to always cover

Programmes go wrong by specifying only the happy path. This list is the minimum:

**Lifecycle:** standard joiner · pre-hire · **rehire** · contractor with end date · **contractor→employee conversion** · internal transfer · manager change · promotion · long leave and return · secondment · standard leaver · **immediate/for-cause leaver** · contractor expiry · death of an employee · bulk reorganisation.

**Access:** request for self · request for another · **request that violates SoD** · high-risk approval chain · time-bound request · emergency/break-glass · bulk request · **request for an application with no automated provisioning**.

**Governance:** manager certification · application owner certification · privileged review · **revocation that fails to execute** · non-responding reviewer · orphan account handling · **entitlement whose owner has left**.

**Operational:** target system unavailable during provisioning · connector credential expiry · duplicate identity detected · **HR feed doesn't arrive** · conflicting attribute values from two sources · rollback of an erroneous bulk change.

{: .warning }
> **The requirements that discriminate between products are almost all in the second and fourth groups** — the failure and exception cases. Every platform does a standard joiner. What separates them is what happens when the target is down for eight hours, when a revocation fails, when two sources disagree, when a bulk change was wrong and must be reversed. **Write those requirements first**, and put them at the front of the PoC. You'll learn more in a day than in a week of happy-path demonstrations.

---

## Prioritisation

Use **MoSCoW** or similar, but apply the discipline that makes it work:

- **Must** — the programme fails without it. Keep this list short: if 80% of requirements are Must, you haven't prioritised, and the vendors know it.
- **Should** — significant value, workaround exists.
- **Could** — nice to have.
- **Won't (this time)** — explicitly out of scope. **Record these**; they prevent scope creep and demonstrate that you considered them.

Then weight for scoring. Ten heavily-weighted requirements that reflect your actual difficulty produce a meaningful comparison; four hundred equally-weighted ones produce noise in which every vendor scores 82%.

---

## Requirements to avoid

| Anti-requirement | Problem |
|:--|:--|
| "Must be user-friendly" | Untestable |
| "Must support industry standards" | Which? For what? |
| "Must integrate with all our systems" | Name them, or it means nothing |
| "Must be scalable" | To what number, at what latency? |
| "Must support Identity Cubes" | Vendor vocabulary — you've pre-selected |
| "Should use AI/ML" | A mechanism, not a requirement. State the outcome you want |
| "Must be best in class" | Unfalsifiable |
| Anything copied from a datasheet | Discriminates for exactly one vendor |

---

## Traceability

Maintain a thread from **business driver → requirement → design decision → test case → evidence.** It answers, at any point: why are we building this, and how will we know it works? It also makes scope changes visible — cutting a requirement now visibly cuts a business driver, which is a conversation the sponsor should have consciously rather than by default.

This does not need a tool. A spreadsheet with five columns, maintained, outperforms an unmaintained requirements management system.

---

## Architect's checklist

- [ ] Are functional requirements written as **use cases with acceptance criteria**?
- [ ] Do they use **your process vocabulary**, not a vendor's?
- [ ] Are the **failure and exception cases** specified, and prioritised into the PoC?
- [ ] Does every non-functional requirement have a **number and an owner**?
- [ ] Is the **Must** list genuinely short?
- [ ] Are requirements **weighted**, so the important ones aren't diluted?
- [ ] Are **out-of-scope** items explicitly recorded?
- [ ] Is there **traceability** from business driver to evidence?
- [ ] Could a competitor's product plausibly meet these, or have you written them around one vendor?

---

**Next:** [Testing](04-testing.md) →

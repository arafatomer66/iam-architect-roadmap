---
title: Architectural Thinking for IAM
parent: 4. Architecture Practice
nav_order: 1
---

# Architectural Thinking for IAM

## Requirements, constraints, and the difference

**Requirements** are what the system must do. **Constraints** are what you cannot change. Junior designers treat constraints as requirements to be argued with; architects treat them as the shape of the problem.

Real constraints in IAM engagements: an existing product with three years left on the contract; a mainframe that cannot be modified; a works council agreement prohibiting session recording; a regulator requiring data residency; a team of four people who will operate this forever; a merger completing in nine months; the fact that the CFO's team will not tolerate an access-request SLA longer than a day during month-end close.

{: .architect }
> **Write the constraints down first, before any design.** Half of all rejected architectures are technically excellent designs that violated a constraint the architect didn't know about or hoped to negotiate away. The other half violated a constraint the architect knew about and hoped nobody would notice. Constraints stated up front become shared context; constraints discovered in review become your credibility problem.

---

## Quality attributes for IAM

The non-functional requirements that actually differentiate designs. For each, know the target *and* who set it:

| Attribute | The IAM-specific question |
|:--|:--|
| **Availability** | What is the target for AM (in the request path) versus IGA (not)? They are not the same number, and treating them as one wastes money or takes risk |
| **Latency** | Authentication adds how many ms? Authorisation per API call? What's the p99, not the average? |
| **Scalability** | Peak authentications per second, not average. What happens on the first Monday of the month? |
| **Revocation latency** | From real-world event to access actually gone. **The number most often unmeasured and most often quoted in policy** |
| **Recoverability** | RTO/RPO for the identity store. If the IdP database is lost, what's the path back? |
| **Auditability** | Can you produce evidence without manual assembly? |
| **Operability** | Can a 4-person team run this? Can they patch it without downtime? |
| **Changeability** | How long to onboard a new application? To change an approval policy? To add an IdP? |
| **Security posture** | Blast radius of compromise, per component |
| **Cost** | Licence + infrastructure + **people**. The third is usually the largest and usually omitted |

The discipline: **give every one of these a number**, and identify which stakeholder owns it. "Highly available" is not a requirement. "99.95% monthly, agreed with the retail platform owner, with degraded read-only authentication acceptable for up to 30 minutes" is.

---

## The trade-offs you will make repeatedly

| Trade-off | Tension |
|:--|:--|
| **Security vs usability** | Every control has a friction cost, and friction produces workarounds that are themselves risk |
| **Centralised vs federated control** | One policy engine is consistent and is a bottleneck and a single point of failure |
| **Build vs buy** | Buying gives support and a roadmap; building gives exact fit and permanent ownership |
| **Standardise vs accommodate** | Forcing 300 applications into one pattern is clean and slow; accommodating each is fast and unmaintainable |
| **Now vs right** | A tactical fix that ships this quarter versus a strategic design that ships next year. **Both answers can be correct** — what's never correct is a tactical fix with no expiry date |
| **Automate vs govern** | Full automation is fast and can propagate an error to 40,000 identities in seconds |
| **Coverage vs depth** | 200 applications with SSO only, or 20 with full governance? Risk-rank, don't guess |

{: .concept }
> **Architecture is the discipline of choosing which problems to have.** Every design has downsides; a design presented without them is either dishonest or under-analysed. The mark of a senior architect is stating the downsides of their own recommendation *before* anyone asks — it converts a defensive review into a shared decision, and it is the single most reliable way to build the credibility that gets your designs approved.

---

## A method that works

### 1. Understand the actual problem

The stated problem is rarely the real one. "We need SailPoint" usually means "we failed an audit on access reviews" — and the audit finding is the problem. Ask: what happens if we do nothing? Who is in pain? What triggered this *now*?

### 2. Map the current state honestly

Not the documented state — the real one. Systems, identity stores, flows, data sources, who does what manually, where the exceptions live. Expect to find things nobody knew about; that's the value of the exercise. See [discovery](../07-delivery/02-discovery.md).

### 3. Define the target state

What should be true in 2–3 years. Not product names — **capabilities and properties**: "every application authenticates through one IdP with phishing-resistant MFA for privileged roles"; "every entitlement has an owner"; "leaver revocation completes within 4 hours, evidenced".

### 4. Identify the gap, and rank it by risk

Not by ease, not by what the tool does well. What's the actual risk reduction per unit of effort?

### 5. Sequence into increments that deliver value alone

**This is the hardest and most valuable step.** Each increment must stand on its own — if the programme is cancelled after increment two, increments one and two must still have been worth doing. Designs that only deliver value at the end don't survive budget cycles.

### 6. Decide, and record why

One [ADR](08-documentation-and-adrs.md) per significant decision, including the options rejected and why. Your future self and your successor need the *reasoning*, not the conclusion.

### 7. Design the run

Who operates this, with what SLAs, what happens at 3am, who clears the exception queue, what it costs annually. A design without an [operating model](../06-business-and-risk/05-operating-model.md) is unfinished.

---

## Thinking tools

**Follow the identity.** Trace one person end to end: hired in HR → identity created → accounts provisioned → logs in → gets a token → accesses data → transfers → some access removed → leaves → revoked → evidence produced. Every gap in that chain is a finding. It's a five-minute exercise that consistently outperforms an hour of architecture discussion.

**Follow the credential.** Where is it created, stored, transmitted, cached, logged, rotated, destroyed? Credentials appearing in logs is a whole class of incident found this way.

**Ask "and then what?"** They request access → approved → provisioned → *and then what?* Nobody ever reviews it. That's the gap.

**Assume the component is down.** For each: is the failure total or partial, does it fail open or closed, how would you know, and what's the workaround? Do this for the IdP, the directory, the PDP, the sync engine, the vault and HR.

**Count the humans.** How many manual steps per joiner? Per access request? Per certification? Multiply by volume. That's your operating cost, and it's the number that justifies automation.

**Ask who is accountable.** For every artefact: the data, the decision, the exception queue, the run cost. Unowned components decay.

---

## Common reasoning failures

| Failure | Looks like |
|:--|:--|
| **Solving for the demo** | A design that works for the happy path and has no answer for partial failure |
| **Product-shaped thinking** | Requirements that describe a product's features rather than the organisation's needs |
| **Ignoring the operating model** | Beautiful architecture, no funded team, decayed within two years |
| **Designing for the org chart you wish existed** | Approval flows that assume owners who don't exist and managers who understand entitlements |
| **Treating the exception as the exception** | The 5% of applications that can't federate is where 60% of the risk lives |
| **Optimising the wrong stage** | Weeks tuning authentication latency while leaver revocation takes eleven days |
| **Big-bang design** | No value until month 30. Cancelled at month 18 |
| **Confusing standards with architecture** | "We use OIDC" is not a design |

---

## Architect's checklist

- [ ] Are the **constraints** written down and agreed before the design?
- [ ] Does every **quality attribute** have a number and a named owner?
- [ ] Is the target state expressed as **capabilities**, not product names?
- [ ] Is the gap ranked by **risk**, not by ease?
- [ ] Does **every increment deliver value on its own**?
- [ ] Have you stated the **downsides of your own recommendation**?
- [ ] For every component: what happens when it's **down**, and does it fail open or closed?
- [ ] Is there a funded **operating model** with named accountabilities?
- [ ] Have you traced **one identity end to end** through the target design?
- [ ] Are the significant decisions recorded as **ADRs** with rejected alternatives?

---

**Next:** [Reference Architecture Blueprints](02-reference-blueprints.md) →

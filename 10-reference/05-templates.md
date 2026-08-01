---
title: Templates
parent: 10. Reference
nav_order: 5
---

# Templates

Copy, adapt, use. Deliberately short — long templates don't get filled in.

---

## Architecture Decision Record

```markdown
# ADR-NNN: <short decision, stated as an outcome>

**Status:** Proposed | Accepted | Superseded by ADR-NNN
**Date:** YYYY-MM-DD
**Deciders:** <names/roles>

## Context
What is true that makes this decision necessary? Include constraints —
technical, organisational, contractual, regulatory. Be honest about the
political ones; they're legitimate context.

## Decision
What we will do. One or two sentences, unambiguous.

## Consequences
+ Positive consequence
+ Positive consequence
− Negative consequence (there are always some — list them)
− Negative consequence, with the mitigation or the ticket tracking it

## Alternatives considered
1. **<Option>** — rejected because …
2. **<Option>** — rejected because …

## Review
Revisit when: <condition or date>
```

---

## Design document

```markdown
# <Name> — Design

**Author · Date · Version · Status**

## 1. Problem and drivers
What happens if we do nothing. Why now.

## 2. Constraints
Technical, organisational, regulatory, commercial. Stated up front.

## 3. Requirements
Functional (as use cases with acceptance criteria).
Non-functional (with numbers and named owners).

## 4. Current state
Honest. Diagrams. What's actually there.

## 5. Target state
Capabilities, not products. Diagrams: identity flow, authentication
sequences, trust boundaries, lifecycle states.

## 6. Gap and sequence
Increments. Each independently valuable. What each delivers.

## 7. Key decisions
Links to ADRs.

## 8. Risks and assumptions
With owners.

## 9. Operating model
Who runs this. What it costs annually.

## 10. Out of scope
What we are explicitly NOT doing, and why.
```

---

## Application onboarding checklist

```markdown
# Application onboarding — <name>

## Classification
- [ ] Business owner (named individual): ______
- [ ] Technical owner: ______
- [ ] Risk tier: 1 / 2 / 3
- [ ] Data classification: ______
- [ ] Regulatory scope: SOX / PCI / GDPR / other: ______
- [ ] User population and type: ______

## Authentication
- [ ] Protocol: SAML / OIDC / proxy / local (justify local)
- [ ] Subject identifier: ______ (stable? non-reassignable?)
- [ ] MFA requirement / AuthnContext requested: ______
- [ ] Session lifetime, idle and absolute: ______
- [ ] Logout: back-channel / front-channel / none
- [ ] Certificate expiry date and renewal owner: ______

## Provisioning
- [ ] Method: SCIM / API / file / ticket / JIT
- [ ] Can current state be read back? (If no → control gap, record it)
- [ ] Deprovisioning behaviour: disable / delete / other: ______
- [ ] Data handling on leaver (files, mailbox, ownership): ______
- [ ] Non-production environment available? (If no → mitigation: ______)

## Governance
- [ ] Entitlements catalogued with business descriptions
- [ ] Entitlement owner(s): ______
- [ ] Risk-rated entitlements identified
- [ ] SoD relevance assessed
- [ ] Certification frequency: ______
- [ ] Included in reconciliation: yes / no

## Operations
- [ ] Connector service account: owner, rotation
- [ ] Monitoring and alerting configured
- [ ] Runbook for provisioning failure
- [ ] Rate limits documented: ______

## Sign-off
- [ ] Business owner · Security · IAM architecture
```

---

## Risk acceptance

```markdown
# Risk acceptance RA-NNN

**Risk:** <scenario: cause → consequence → scale>
**Raised by / Date:**

**Impact if realised:** <quantified where possible>
**Likelihood:** <with reasoning>

**Why not remediated now:** <cost, timing, business necessity>

**Compensating controls:**
| Control | Who performs it | Frequency | Where the evidence lives |
|---|---|---|---|

**Accepted by:** <named individual, role — senior enough to own the consequence>
**Accepted on:** YYYY-MM-DD
**Expires:** YYYY-MM-DD   ← mandatory
**Review owner:** ______

**On expiry:** remediate, or re-accept with fresh justification.
```

---

## Runbook

```markdown
# Runbook: <situation>

**Owner · Last tested (date) · Severity**

## Am I in the right place?
Symptoms: …
Confirm with: <specific check/command>
If instead you see X, go to <other runbook>.

## Prerequisites
- Access required: ______
- Approval required: ______ (and how to get it out of hours)
- Tools/credentials: ______

## Steps
1. <Explicit command or screen>
   Expected result: ______
2. …
3. **DECISION POINT:** if <condition>, go to step N. Otherwise continue.

## Verify
How you know it worked: ______

## If it doesn't work
Escalate to: <name, role, contact — including out of hours>

## Afterwards
- [ ] Record in the incident log
- [ ] Rotate anything used (e.g. break-glass credentials)
- [ ] Raise a follow-up if a fix is needed

> Keep an offline copy. This may be needed when SSO is unavailable.
```

---

## Access review campaign brief

```markdown
# Certification campaign — <name>

**Scope:** <which identities, which entitlements, which applications>
**Rationale for scope:** <risk-based justification — auditors will ask>
**Reviewers:** manager / application owner / role owner
**Period:** <start> to <end>
**Escalation:** non-response at day N → <manager> → day M → <executive>

## Reviewer aids
- [ ] Business-language descriptions for every item
- [ ] Last-used date shown
- [ ] Peer comparison shown
- [ ] Risk rating shown

## Success criteria
- Completion rate target: ___%
- **Revocation execution rate target: ___%**   ← the one that proves the control
- Evidence exportable without manual assembly

## Post-campaign
- [ ] Revocations verified as executed
- [ ] Non-responders escalated and recorded
- [ ] Items always revoked → review at source
- [ ] Lessons for the next campaign
```

---

## Interview scorecard (for hiring architects)

```markdown
Candidate · Date · Interviewer

| Dimension | Evidence observed | 1–5 |
|---|---|---|
| Clarifies before designing | | |
| Reasons vs recalls | | |
| Knows failure modes | | |
| Volunteers trade-offs | | |
| Communicates at multiple altitudes | | |
| Governance depth (not just protocols) | | |
| Business/risk translation | | |
| Owns a past mistake credibly | | |
| Says "I don't know" appropriately | | |

Would I want them designing something I have to live with? Y / N — why:
```

---

**Next:** [Reading & Following](06-reading-list.md) →

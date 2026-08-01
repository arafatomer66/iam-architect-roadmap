---
title: 6. Business & Risk
nav_order: 7
has_children: true
---

# Stage 6 — Business & Risk

> *"IAM is not just about access. It's about impact."*

## The stage that separates engineers from architects

An architect who cannot express a design in the language of **risk, cost, audit findings and time-to-productivity** will lose every funding argument to someone who can — regardless of who is technically right.

This is the most commonly missing competency in people who are otherwise ready for the role. It is also the most learnable, because it's mostly a matter of translation.

{: .concept }
> **Nobody funds identity because it is correct. They fund it because of what it prevents, enables, or proves.** Prevents: a breach, a fine, an audit finding. Enables: a new hire productive on day one, a merger completed in months, a customer product launched. Proves: to a regulator, an auditor, a board or a customer that access is controlled. **Every design you propose should be expressible in one of those three sentences**, and if it isn't, either you haven't found the reason or it shouldn't be funded.

## Pages in this stage

| # | Page | What it gives you |
|:--|:--|:--|
| 1 | [Business Alignment](01-business-alignment.md) | How to connect identity work to business outcomes |
| 2 | [Identity Risk](02-identity-risk.md) | Quantifying and communicating risk credibly |
| 3 | [Compliance & Regulation](03-compliance.md) | SOX, GDPR, NIS2, DORA, PCI, HIPAA, ISO, NIST — the identity controls in each |
| 4 | [Zero Trust as a Business Programme](04-zero-trust-business.md) | Selling and sequencing the thing everyone claims to be doing |
| 5 | [Operating Model & Org Design](05-operating-model.md) | Who runs this, in what structure, at what cost |
| 6 | [Metrics, KPIs & Reporting](06-metrics.md) | What to measure and what to show whom |
| 7 | [Business Case & Stakeholders](07-stakeholders.md) | Building the case, and getting it approved by people who disagree |

## The translation table

Keep this in your head. It is the single most useful thing in this stage:

| You would say | They hear | Say instead |
|:--|:--|:--|
| "We'll implement SCIM provisioning" | Cost | "New hires will be productive on day one instead of day eight; that's 6,000 hours a year" |
| "We need to fix our SoD model" | Bureaucracy | "This closes the audit finding that put us on the regulator's remediation list" |
| "We should adopt phishing-resistant MFA" | Friction | "This removes the attack that caused three of our last four incidents, and it's *faster* to use" |
| "We need role mining" | Consultants | "We can't answer who has access to the payment system without three days of manual work" |
| "The IdP needs multi-region HA" | Infrastructure spend | "An hour of authentication downtime stops 12,000 people working. That's €X" |
| "We should govern non-human identities" | Scope creep | "We have 40,000 credentials with no owner and no expiry. Any one of them is a way in" |

---

**Start:** [Business Alignment](01-business-alignment.md) →

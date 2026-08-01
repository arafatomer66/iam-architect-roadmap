---
title: 4. Architecture Practice
nav_order: 5
has_children: true
---

# Stage 4 — Architecture Practice

## The stage that makes you an architect

Stages 1–3 gave you the material. This stage is about the **job**: producing designs that are correct, sequenced, defensible and durable — and getting them built by people who don't report to you.

This is the gap that stops most senior engineers. They can answer *how does SAML work?* perfectly and stall on *given these five constraints, three of which contradict each other, what should we build first and why?*

{: .concept }
> **An architecture is a set of decisions, plus the reasoning that justifies them, plus a path from here to there.** A target-state diagram with no sequence is a wish. A sequence with no reasoning is a project plan someone will overturn in the first steering committee. A decision without a documented alternative is an opinion. All three parts are the deliverable.

## Pages in this stage

| # | Page | The skill it builds |
|:--|:--|:--|
| 1 | [Architectural Thinking for IAM](01-architectural-thinking.md) | Requirements, constraints, trade-offs, quality attributes, how to reason |
| 2 | [Reference Architecture Blueprints](02-reference-blueprints.md) | Concrete target states for common situations |
| 3 | [Integration Patterns](03-integration-patterns.md) | Connecting anything to anything, and surviving partial failure |
| 4 | [Migration & Coexistence](04-migration-and-coexistence.md) | Getting from the current mess to the target without a big bang |
| 5 | [HA, DR & Scale](05-ha-dr-and-scale.md) | Designing for the day something breaks |
| 6 | [Securing the IAM Platform Itself](06-securing-iam.md) | The IdP is the crown jewel; protect it accordingly |
| 7 | [Anti-Patterns](07-anti-patterns.md) | The tempting mistakes, and why they're tempting |
| 8 | [Diagrams, ADRs & Documentation](08-documentation-and-adrs.md) | Making decisions visible, reviewable and durable |

## The four questions

Every IAM design decision comes down to some combination of:

1. **Where does the truth live?** Authoritative source, correlation, conflict resolution.
2. **When is the decision made?** Design time (roles), request time (approval), runtime (policy), or after the fact (certification).
3. **What happens when it fails?** Fail open, fail closed, degrade, queue. Every component, decided deliberately.
4. **Who is accountable?** For the data, for the decision, for the exception queue, for the run cost.

If you can answer those four for every component in your design, you have an architecture. If you can't, you have a diagram.

---

**Start:** [Architectural Thinking for IAM](01-architectural-thinking.md) →

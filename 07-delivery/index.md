---
title: 7. Delivery & Operations
nav_order: 8
has_children: true
---

# Stage 7 — Delivery & Operations

## Why an architect owns this

The best architecture that cannot be delivered is worth less than a decent one that ships. And an architecture that ships without an operable run phase decays within two years.

Architects who treat delivery as "someone else's problem" produce designs that get quietly abandoned during implementation, when a project manager under pressure makes a scope decision that guts the design's intent — because nobody explained which parts were load-bearing.

{: .concept }
> **Your design has a shelf life measured in decisions made by other people.** Every scope cut, every "we'll do that in phase two", every workaround under deadline pressure erodes it. Staying engaged through delivery isn't project management encroaching on architecture — it's **defending the reasoning** when it meets reality. And where reality is right, it's updating the design honestly rather than pretending the plan survived.

## Pages in this stage

| # | Page | Covers |
|:--|:--|:--|
| 1 | [IAM Programme Delivery](01-programme-delivery.md) | Phasing, governance, why IAM programmes fail |
| 2 | [Discovery & Assessment](02-discovery.md) | Finding out what's actually there |
| 3 | [Requirements & Use Cases](03-requirements.md) | Writing requirements that survive contact with vendors |
| 4 | [Testing](04-testing.md) | Test strategy for identity, including the negative cases |
| 5 | [Run & Operations](05-run-and-operations.md) | Runbooks, monitoring, the daily reality |
| 6 | [Identity Incident Response](06-incident-response.md) | When an identity is compromised, right now |

## The delivery truths

**1. Sequence beats scope.** A programme delivering leaver automation in month four earns the credibility to continue. One that delivers nothing until month thirty gets cancelled at month eighteen.

**2. Data before tooling.** Every hour spent on data quality before implementation saves several during it.

**3. The hard integrations don't get easier by being deferred.** Attempt one early to learn what "hard" means in your estate — before you've committed to a timeline built on the easy ones.

**4. The business does the governance work.** IAM builds the mechanism; application owners make the decisions. If the owners don't exist, that's your first project.

**5. Nothing is done until it's reconciled.** Provisioning that isn't verified against the target's actual state is automation, not control.

**6. Go-live is the start.** Design and fund the run phase before you launch, because afterwards the budget conversation is much harder.

---

**Start:** [IAM Programme Delivery](01-programme-delivery.md) →

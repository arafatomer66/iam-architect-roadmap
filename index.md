---
title: Home
layout: landing
nav_order: 0
description: "A concept-first roadmap to becoming an IAM Architect — principles, protocols and business thinking, not product training."
---

## What this is, precisely

An IAM architect is the person who can walk into a room where nobody agrees and answer questions like:

- *Should this application federate, or should we provision accounts into it?*
- *Who is allowed to say that this person should have this access — and how do we prove it in an audit twelve months from now?*
- *We are acquiring a company with 4,000 employees and its own directory. What happens on day one, day thirty, and day four hundred?*
- *The developers want to issue a token to an AI agent acting on a user's behalf. What is the blast radius when that token leaks?*

None of those are product questions. They are questions about **trust, data, lifecycle, risk and organisational reality**. Products are how you implement the answer — they are never the answer.

This repository is the curriculum for that skill set: **vendor-neutral by design and vendor-aware by necessity**. Every concept is taught on its own terms first, then mapped onto how SailPoint, One Identity, Ping Identity, Okta, Microsoft Entra ID, Saviynt, ForgeRock, CyberArk and Keycloak express it — because you will have to speak those dialects on real projects.

## How every page is built

1. **The concept** — what problem in the world made someone invent this. A protocol without its problem is trivia.
2. **The mechanics** — how it works, at the depth an architect must carry without notes.
3. **The architect's lens** — trade-offs, failure modes, and an **Architect's checklist** of questions you must be able to answer before signing off a design.

Vendor detail is isolated in *"In the products"* callouts and in [Stage 5](05-platform-landscape/), so it can go stale without rotting the durable content around it.

## Start where you are

| If you are… | Start at |
|:--|:--|
| New to IAM entirely | [The 12-month learning plan](00-start-here/05-learning-plan.md) |
| An IAM engineer moving up | [Self-assessment](00-start-here/04-self-assessment.md), then [Architecture Practice](04-architecture-practice/) |
| A developer who knows OAuth but not enterprise lifecycle | [Joiner-Mover-Leaver](02-identity-fundamentals/14-joiner-mover-leaver.md) |
| From security or GRC | [Stage 1](01-it-fundamentals/), then the protocols |
| Interviewing in two weeks | [Interview prep](09-practice/04-interview-prep.md) |
| Choosing or replacing a platform | [The vendor-neutral map](05-platform-landscape/00-vendor-neutral-map.md) |

Full reading paths: [How to use this repo](00-start-here/03-how-to-use-this-repo.md).

---

*Independent educational project, licensed CC BY 4.0. Trademarks belong to their owners; nothing here is endorsed by any vendor. Always verify product specifics against current vendor documentation before relying on them in a design.*

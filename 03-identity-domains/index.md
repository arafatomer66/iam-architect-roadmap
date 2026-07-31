---
title: 3. Identity Domains
nav_order: 4
has_children: true
---

# Stage 3 — Identity Domains

## The same concepts, wildly different physics

Stage 2 taught the mechanisms. This stage is about the fact that **the same mechanism behaves completely differently depending on who — or what — the subject is.**

| Domain | Population | Who creates them | Lifecycle trigger | Dominant concern |
|:--|:--|:--|:--|:--|
| **Workforce** | 10³–10⁵ | HR | Employment events | Governance & audit |
| **CIAM** | 10⁵–10⁸ | The user, themselves | Self-service | Scale, UX, privacy, fraud |
| **B2B / partner** | 10³–10⁶ | The partner organisation | Contractual | Delegated administration & trust boundaries |
| **Non-human** | 10⁴–10⁶ | Developers, automatically | Deployment | Discovery, ownership, rotation |
| **Workload** | 10³–10⁷, ephemeral | The platform | Scheduling | Attestation, secretless auth |
| **OT / IoT** | 10³–10⁷ | Manufacturing / installation | Physical | Longevity, constrained devices, safety |

{: .concept }
> **An architecture that works brilliantly in one domain can be actively wrong in another.** Quarterly access certification is a sound workforce control and is meaningless for 40 million consumers. A rigorous HR-driven joiner process is irrelevant for a Kubernetes pod that lives for 90 seconds. Password policy is a real control for humans and a category error for a service account that should have no password at all. **Know which domain you're designing for, and say so explicitly** — many bad IAM designs are simply the right design for the wrong domain.

## Pages in this stage

| # | Page | The defining challenge |
|:--|:--|:--|
| 1 | [Workforce Identity](01-workforce-identity.md) | Governance, audit and organisational change |
| 2 | [Customer Identity (CIAM)](02-ciam.md) | Scale, conversion, privacy and fraud, simultaneously |
| 3 | [B2B & Partner Identity](03-b2b-identity.md) | Trust across organisational boundaries, delegated administration |
| 4 | [Non-Human Identities & Secrets](04-non-human-identities.md) | You cannot govern what nobody knows exists |
| 5 | [Workload & Cloud Identity](05-workload-identity.md) | Authentication without a stored secret |
| 6 | [OT, IoT & Edge Identity](06-ot-and-iot.md) | 20-year lifespans, constrained hardware, safety consequences |

## The trajectory

Human identity grows with headcount — linearly, slowly, predictably. **Non-human identity grows with deployments**, which means it grows with automation, microservices, cloud adoption and now AI agents. In most enterprises NHIs already outnumber humans by 10:1 to 100:1, and they are provisioned by developers rather than by HR, have no natural leaver event, and frequently hold more privilege than the humans.

If your identity programme covers only the humans, it covers a shrinking minority of your identity risk.

---

**Start:** [Workforce Identity](01-workforce-identity.md) →

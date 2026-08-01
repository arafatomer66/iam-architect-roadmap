---
title: Design Exercises
parent: 9. Practice
nav_order: 2
---

# Design Exercises

Problems to solve yourself. Each has **evaluation criteria** rather than an answer — good designs vary, but strong ones cover the same ground.

Work in writing. Aim for 1–3 pages per exercise. Time-box them.

---

## Exercise 1 — The JML design (90 minutes)

> Design joiner-mover-leaver for a 6,000-person European engineering company. Populations: 4,200 employees (SAP SuccessFactors), 900 contractors (managed in a procurement system with no end dates recorded), 500 staff of a recently acquired subsidiary with its own HR system, and 400 agency workers onboarded by site managers via email. Systems: AD, Entra, SAP, a PLM system, a mainframe, 60 SaaS applications, and a badge/physical access system.

**Your design must cover:**
- Authoritative source per population, and what you do about the two that don't have one
- The joiner, mover and leaver flows, including credential delivery
- At least eight edge cases, handled explicitly
- Revocation SLAs by risk, and how each is met technically
- What happens when a target system is unavailable
- How the physical access system fits

**Evaluation criteria:**

| Strong design | Weak design |
|:--|:--|
| Creates an authoritative source for agency workers and contractor end dates | Assumes the data will be provided |
| Mover includes a targeted review with auto-revoke on timeout | Mover is additive only |
| Leaver disables authentication first, then revokes entitlements | Revokes entitlements first, leaving a login window |
| Handles sessions, tokens and shared secrets, not just accounts | Stops at "disable the account" |
| Explicit handling of rehire, contractor→employee, dual role, long leave, death, subsidiary staff | Happy path only |
| Physical access included in both joiner and leaver | Ignored |
| Mainframe handled by ticket with reconciliation | Assumed automatable, or ignored |
| SLA differentiated by risk, with a stated mechanism for each | One SLA for everything |

---

## Exercise 2 — The federation decision (45 minutes)

> Your company is onboarding 40 distribution partners over 12 months. Partners range from 5-person operations with no IT to 2,000-person organisations with their own Entra tenant. They need access to an ordering portal and, for some, to inventory data. Contracts are typically 3 years with annual review.

**Decide and justify:** the identity model(s), how you learn about leavers, the tenancy and delegated administration model, and what happens at contract end.

**Evaluation criteria:**
- Does the design use **more than one model**, matched to partner maturity? (Federation for the large ones; delegated administration for the small ones; sponsorship for individuals.)
- Is there a **named mechanism** for learning about leavers, not an aspiration? Periodic attestation with suspension for non-response is the expected answer.
- Is the **contract modelled as an object** whose end cascades to its identities?
- Are **asserted identifiers scoped** for federated partners?
- Is there a per-partner **kill switch**?
- Is tenant isolation enforced at the **data layer**?
- Does the answer acknowledge that small partners **cannot** federate, rather than assuming they will?

---

## Exercise 3 — The token architecture (60 minutes)

> Design the token and session architecture for a healthcare platform. Actors: clinicians (web + mobile), patients (web + mobile), integration partners (server-to-server), and internal microservices (approximately 40). Regulatory requirement: access revocation within 5 minutes of a change in a clinician's authorisation. Clinical staff must not be logged out mid-consultation.

**Specify:** token types and lifetimes, session model per actor, revocation mechanism, how the 5-minute requirement is actually met, and the service-to-service model.

**Evaluation criteria:**
- Does the design **notice the tension** between 5-minute revocation and not interrupting consultations, and resolve it deliberately?
- Are access token lifetimes reconciled with the revocation requirement — **or is introspection used** for the paths that need it?
- Does service-to-service use **token exchange** preserving user identity, rather than losing it?
- Are patient and clinician session models **different**, appropriately?
- Is step-up specified for sensitive actions?
- Is there a mechanism for **emergency access** (break-glass to patient records), with alerting and post-hoc review? *(In healthcare this is a named regulatory requirement — a strong answer includes it unprompted.)*
- Are mobile clients using the system browser and PKCE?

---

## Exercise 4 — The privileged access design (60 minutes)

> A utility company. 380 IT administrators, 40 OT engineers, 12 external vendors requiring remote access to control systems, and a Tier 0 population of 25. Regulatory environment requires session recording for privileged access to critical infrastructure. A works council must approve any employee monitoring. Availability of the control systems is paramount — a control action must never be delayed by an identity system.

**Design:** the tiering model, the access paths for each population, break-glass, and how you handle the works council and the availability constraint.

**Evaluation criteria:**
- Is **vendor remote access** brokered, time-bound and recorded — with no standing access and no direct network path?
- Does the design **prioritise availability and safety** in the OT context, and say so explicitly?
- Does anything proposed add **latency to a control action**? (It must not.)
- Is **break-glass** designed for the case where the PAM system itself is unavailable?
- Is the **works council** engaged before deployment, with a documented scope of monitoring?
- Are **shared operator consoles** addressed realistically rather than banned?
- Is Tier 0 separated with PAWs and separate credentials?

---

## Exercise 5 — The authorisation model (45 minutes)

> A B2B SaaS platform. Customers are organisations; each has users with roles. Some customers want to define their own roles. Data must be strictly isolated per customer. Some users belong to multiple customer organisations (consultants). One customer requires that their users' access be governed by *their* IdP's group membership. Volume: 4,000 customer organisations, 400,000 users.

**Decide:** the authorisation model(s), where enforcement happens, how multi-org membership works, and how customer-defined roles are constrained.

**Evaluation criteria:**
- Is **tenant isolation enforced structurally at the data layer**, not per-endpoint?
- Does the model handle **multi-org users** without requiring separate accounts?
- Are **customer-defined roles** constrained to a permission set the customer cannot exceed?
- Is the mapping from an external IdP's groups **scoped to that customer only**?
- Is the model a **sensible hybrid** (RBAC for roles, tenant as a mandatory dimension) rather than forcing everything into one paradigm?
- Does it scale — is the authorisation check a per-request database query per object, or something workable at volume?

---

## Exercise 6 — The one-page architecture (30 minutes)

> You have one page and fifteen minutes with the CEO. Present the identity architecture and the investment case for a programme you designed in an earlier exercise.

**Evaluation criteria:**
- **No protocol names.**
- Three numbers or fewer, each with a trend or a target.
- The ask is **specific**: amount, for what, by when.
- The consequence of doing nothing is stated.
- A trade-off is named, so it reads as a decision rather than a pitch.
- It fits on one page. *(If it doesn't, you haven't decided what matters.)*

---

## Exercise 7 — The post-incident review (45 minutes)

> An attacker phished a helpdesk agent, called the service desk impersonating a senior administrator, had that administrator's MFA reset, authenticated, and had Global Administrator for six days. They then left. You have been asked what to do.

**Produce:** the immediate response, the persistence hunt, the recovery plan, and the three architectural changes you would fund.

**Evaluation criteria:**
- Does the response **revoke sessions and tokens**, not just disable accounts?
- Does the **persistence hunt** cover new signing keys, new federation trusts, consent grants, CA exclusions, new service principal credentials, role assignments and MFA registrations?
- Does it recognise that a **configuration baseline** is needed to answer "is this legitimate?"
- Does recovery include **rotating everything the identity could reach**?
- Are the architectural changes the *right* ones — helpdesk verification process, detections for configuration persistence, JIT instead of standing Global Admin — rather than generic "more training"?
- Is there an honest statement about **what you cannot determine** with the logs available?

---

## Self-marking

For each exercise, score yourself:

| | |
|:--|:--|
| **Did I cover the failure cases**, or only the happy path? |
| **Did I state trade-offs**, including the downsides of my own choice? |
| **Did I answer "what happens when this component is down?"** |
| **Did I name who is accountable** for each part? |
| **Could someone implement this** from what I wrote? |
| **Did I say what I'm explicitly not doing?** |

Missing the same criterion across several exercises tells you where to go back in Stages 1–8.

---

**Next:** [Whiteboard Scenarios](03-whiteboard-scenarios.md) →

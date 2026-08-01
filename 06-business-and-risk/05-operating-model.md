---
title: Operating Model & Org Design
parent: 6. Business & Risk
nav_order: 5
---

# Operating Model & Org Design

## The part of the architecture that isn't technical

A design specifies what gets built. An **operating model** specifies who runs it, how decisions get made, what happens when it breaks, and what it costs every year thereafter.

{: .concept }
> **An IAM platform without a funded operating model decays into an expensive report generator within eighteen months.** The pattern is completely predictable: the project team disbands, no one onboards new applications, the exception queue grows unread, the role model drifts, certification campaigns are run by whoever is available, and three years later someone proposes replacing the platform — which does not fix the actual problem. **The operating model is part of the architecture. Design it, cost it, and get it funded before go-live**, because after go-live the budget conversation is much harder.

---

## The functions that must exist

Whoever performs them, these jobs are permanent:

| Function | What it does | Frequency |
|:--|:--|:--|
| **Platform operations** | Availability, patching, upgrades, capacity, certificates | Continuous |
| **Integration engineering** | Onboarding new applications, maintaining connectors | Continuous — a permanent stream, not a project |
| **Exception handling** | Failed provisioning, orphan accounts, uncorrelated identities, quarantined records | **Daily** |
| **Access administration** | Requests that can't be automated, emergency access, support | Daily |
| **Governance operations** | Certification campaigns, SoD violations, chasing non-responders | Cyclical |
| **Data stewardship** | Chasing source system owners on quality; managing the register | Weekly |
| **Catalogue & role management** | Descriptions, owners, retirement of unused roles | Ongoing |
| **Architecture & standards** | Design, patterns, standards, decisions | Ongoing |
| **Audit support** | Evidence production, auditor liaison | Cyclical |
| **Incident response** | Identity-specific incidents ([IR](../07-delivery/06-incident-response.md)) | On demand |
| **Vendor management** | Licences, support, roadmap, renewals | Ongoing |

**The two most commonly unfunded are exception handling and integration engineering** — the first because it's invisible until it's a crisis, the second because it's assumed to end with the project. Neither ends.

---

## Sizing

Rough starting points for a mid-to-large enterprise (10,000–30,000 identities, 50–150 governed applications). Validate against your own volumes; these are for provoking a conversation, not for quoting:

| Function | Indicative FTE |
|:--|:--|
| Platform operations | 1–2 |
| Integration engineering | 2–4 (drops if the application onboarding rate falls, which it usually doesn't) |
| Exception handling + access administration | 1–3, scaling with population and automation quality |
| Governance operations | 1–2 |
| Architecture | 1 (plus fractional senior architecture) |
| Product ownership | 0.5–1 |

**The strongest lever on this number is automation quality.** A platform where 8% of provisioning operations fail needs several people clearing queues; one where 0.5% fail needs a fraction of a person. Which means investment in reliability has a directly calculable payroll return — a useful argument when quality work needs justifying.

---

## Organisational placement

| Model | Works when | Watch for |
|:--|:--|:--|
| **Under the CISO** | Security-driven, risk-led organisations | IAM seen as a control function that blocks; weaker relationship with IT delivery |
| **Under IT infrastructure** | Operationally mature IT | Governance under-weighted; risk voice quieter |
| **Under the CIO as its own function** | Large organisations where IAM is strategic | Needs a strong leader to avoid isolation |
| **Split: governance under CISO, operations under IT** | Common in large enterprises | **Requires explicit decision rights**, or the two halves conflict continuously |
| **Federated to product teams with a central platform** | Product-led engineering organisations | Central team must be an enabler, not a gate ([anti-patterns](../04-architecture-practice/07-anti-patterns.md)) |

There is no universally right answer. What matters far more than the reporting line: **decision rights are explicit**, the function has a route to executive attention, and it is not exclusively judged on ticket throughput — because a team measured on tickets closed will optimise for closing tickets rather than for removing the need for them.

---

## Decision rights

Write these down. Ambiguity here is the source of most IAM organisational friction:

| Decision | Typically owned by | Consulted |
|:--|:--|:--|
| Identity architecture and standards | IAM architecture | Security, IT, business |
| Who may access an application | **The application/data owner** | IAM (mechanism), manager |
| What an entitlement means and its risk | Application owner | IAM |
| Whether an exception is granted | Risk/security, per policy | IAM, business |
| Role definitions | Business role owner | IAM |
| Platform change and release | IAM operations | Change management |
| Onboarding priority | IAM product owner with business input | — |
| Accepting an identity risk | **A named business/executive owner** | IAM, security |

{: .architect }
> **The single most valuable organisational artefact in an IAM programme is a maintained list of application and entitlement owners.** Without it: certifications can't be routed, requests can't be approved by someone who understands them, exceptions have no decision-maker, and everything escalates to the IAM team — who then make business decisions they have no mandate for. Building and maintaining that list is unglamorous, political, and worth more than most technical work you will do. Treat owner-record staleness as a defect: when an owner leaves, their applications must be reassigned as part of the [leaver process](../02-identity-fundamentals/14-joiner-mover-leaver.md).

---

## Service management

Treat IAM as a service with published expectations:

| Service | Target | Measured how |
|:--|:--|:--|
| Access request (standard) | 90% in 24h | Request → provisioned |
| Access request (privileged) | 90% in 4h | Same |
| Joiner readiness | 100% by start date | Provisioned before day one |
| Leaver revocation (standard) | 4h from HR event | End to end |
| Leaver revocation (privileged) | 15 min | End to end |
| Password/credential reset | Self-service ≥85% | Deflection rate |
| Application onboarding | Standard 10 days, complex 30 | Request → in production |
| Certification campaign | Completed within window | Completion + revocation execution |

Publishing these does two things: it sets expectations so people stop escalating, and it **exposes the gap between the policy and reality**, which is the argument for investment.

---

## Building the team

Roles worth naming explicitly, because organisations often miss the last two:

- **IAM architect** — design, standards, decisions.
- **IAM engineers** — build and integrate.
- **IAM analysts/operators** — run the process, clear exceptions.
- **IAM product owner** — priorities, roadmap, stakeholder management. **Frequently missing**, and its absence is why IAM teams get pulled in every direction.
- **Identity data steward** — owns the data quality conversation with source systems. **Almost always missing**, and it's the role that unblocks the most.

On hiring: IAM skills are scarce, and the market rewards people who have them. Growing from adjacent skills is often faster than hiring — infrastructure and AD people bring fundamentals; developers bring protocols and automation; GRC people bring the control language. All three need a year of deliberate development. Budget for training, and expect that the people you develop will become attractive to other employers, which is a retention question rather than a reason not to develop them.

---

## Architect's checklist

- [ ] Is there a **funded operating model**, agreed before go-live?
- [ ] Are all eleven functions above **assigned to someone**?
- [ ] Who clears the **exception queue daily**, and what is its current depth and age?
- [ ] Is **integration engineering funded permanently**, not just for the project?
- [ ] Are **decision rights documented**, particularly "who decides who gets access"?
- [ ] Is there a maintained list of **application and entitlement owners**, refreshed when owners leave?
- [ ] Are **service levels published**, and measured end to end?
- [ ] Is there an **IAM product owner** and a **data steward**?
- [ ] Is the team measured on **outcomes**, not just ticket throughput?
- [ ] What is the annual **run cost**, and does the sponsor know it?

---

**Next:** [Metrics, KPIs & Reporting](06-metrics.md) →

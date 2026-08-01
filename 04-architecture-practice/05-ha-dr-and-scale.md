---
title: HA, DR & Scale
parent: 4. Architecture Practice
nav_order: 5
---

# High Availability, Disaster Recovery & Scale

## Identity is the dependency of everything

When the identity provider is down, **nobody works** — not the call centre, not the warehouse, not the developers, not the executives. Few systems have that property. It changes the availability conversation from "how much can we afford?" to "what is the cost per minute of the entire organisation stopping?"

{: .concept }
> **Not all identity components have the same availability requirement, and treating them as one is how money gets wasted or risk gets taken.** Access management is in the request path: if it's down, work stops. IGA is not: if it's down for four hours, some joiners are late and some requests queue — annoying, not existential. PAM sits between: it's not in the normal path, but if it's down during an incident you cannot administer anything. **Design three tiers, not one.**

| Component | Typical target | Impact of outage |
|:--|:--|:--|
| **Access management / IdP** | 99.95%+ | Total work stoppage |
| **Directory** | 99.95%+ | Authentication and application lookups fail |
| **PAM** | 99.9% + break-glass | Administration blocked, exactly when you need it |
| **IGA** | 99.5% | Provisioning delayed, requests queued |
| **Sync engine** | 99.5% | Data staleness that compounds silently |

---

## Failure domains

Design against the domain, not the component:

- **Instance** — a node fails. Handled by load balancing and health checks.
- **Zone** — a datacentre or availability zone. Handled by multi-AZ deployment.
- **Region** — a whole region. Handled by multi-region, which raises data residency and replication-lag questions.
- **Dependency** — the database, DNS, the HSM, a certificate, an upstream IdP. Frequently the actual cause.
- **Configuration** — someone deployed a bad policy. **The most common real cause of identity outages**, and the one multi-region does nothing to help.
- **Vendor** — the SaaS IdP itself has an incident. You cannot engineer around it; you can only plan for it.

{: .warning }
> **The most likely identity outage is a change you made, not hardware failing.** A conditional access policy applied to the wrong group; a certificate replaced without overlap; a firewall rule; a sync scope edited by one character. This is why change control, staged rollout, policy simulation ("what-if" tooling) and fast rollback matter more than another datacentre. Ask about the last three outages in any estate — they will almost all be changes.

---

## Patterns

**Active-active** — all sites serve traffic. Best availability and no failover event to get wrong, but requires session state that works across sites (shared store, or stateless tokens) and tolerance of replication lag for writes.

**Active-passive** — standby site promoted on failure. Simpler, but the failover is a procedure, and untested procedures fail. If you use this, **test the failover on a schedule**, not on a theory.

**Read-local / write-global** — reads served from any region, writes routed to a primary. Fits directories well, since identity workloads are read-dominant.

**Cached / degraded authentication** — the pattern most worth designing deliberately. Can users with an existing session continue working while the IdP is unavailable? Can a token be issued from cached data? Longer token lifetimes increase resilience and reduce revocation responsiveness — that is a **conscious trade-off between availability and security**, and it should be made explicitly rather than inherited from a default.

---

## Break-glass

Whatever the architecture, you need a path that works when identity itself is broken:

- Cloud-native or local accounts, excluded from federation and conditional access.
- Credentials split and physically sealed, or in a vault with a **different failure domain** from the primary identity estate.
- Loud alerting on any use, over channels that survive the outage you're breaking glass for.
- Tested on a schedule; rotated after every use and every test.
- **Documented in a form retrievable when systems are down** — a runbook stored only in the wiki that requires SSO to read is a well-known and entirely avoidable failure.

---

## Disaster recovery

| Question | Must have a specific answer |
|:--|:--|
| **RPO** — acceptable data loss | Minutes for identity data. Losing an hour of provisioning means an hour of unknown state |
| **RTO** — acceptable downtime | Usually minutes for AM, hours for IGA |
| **Backup scope** | Identity store, configuration, policies, **secrets and keys**, certificates. Configuration is the one most often missed |
| **Restore testing** | Untested backups are hopes. Test annually at minimum |
| **Key material** | If the HSM or KMS is lost, can you decrypt the backup? Where is the key escrow? |
| **Order of restoration** | Identity comes back **first** — you can't administer anything else without it. This should be explicit in the organisation's overall DR plan |

{: .architect }
> **A backup of the identity store without the configuration is close to useless.** Restoring users but not the federation trusts, policies, connector definitions, workflows, role model and signing keys leaves you with a directory and no working identity service. Include configuration in backups, version it in source control where the product allows, and — importantly — **verify that a restore produces a working system**, not merely a restored database. Test the whole path once a year.

---

## Scale

**Model peaks, not averages.** Authentication load is spiky: Monday 09:00, the first day after a holiday, after a mass password reset, during an incident, and — the one that catches people — **immediately after an outage**, when everyone retries at once. Design for the thundering herd; it is a real failure mode where a recovering service is knocked over by the recovery.

Numbers to establish for any design:

- Peak authentications per second (and per minute — bursts matter more than sustained rates).
- Token validation rate at resource servers.
- Directory queries per second, and whether they hit **indexed** attributes.
- Provisioning operations per hour during a bulk event (reorganisation, mass onboarding).
- Certification campaign load — thousands of reviewers hitting the UI in the last two days before the deadline, every time.

**Where the bottlenecks actually appear:** directory search on unindexed attributes; database connection pools; session store capacity; slow-by-design password hashing under a login storm (correct, but must be capacity-planned); rate limits at downstream SaaS during bulk provisioning; and JWKS endpoint load if consumers don't cache.

**Cost of scale** is not just infrastructure: per-user licensing means growth has a linear cost that should appear in the [business case](../06-business-and-risk/07-stakeholders.md).

---

## Observability

You cannot operate what you cannot see:

- **Golden signals per component**: authentication success/failure rate, latency percentiles, token issuance rate, provisioning queue depth, sync lag, error classes.
- **Synthetic transactions.** A robot that logs in every minute from multiple locations and alerts on failure. **This is the single highest-value monitoring investment in an identity estate** — it detects what infrastructure metrics miss, including "the login page returns 200 but authentication is broken".
- **Business-level alerts**: "joiners not provisioned in the last hour", "certification campaign at 12% with two days remaining", "sync last completed 6 hours ago".
- **Dependency monitoring**: certificate expiry, DNS, upstream IdPs, HSM health, database replication lag.

---

## Architect's checklist

- [ ] Does each identity component have its **own availability target**, agreed with the business?
- [ ] What is the **cost per minute** of an authentication outage? Does anyone know?
- [ ] Which **failure domains** are covered — instance, zone, region, dependency, configuration?
- [ ] Is there **policy simulation and staged rollout** for configuration changes?
- [ ] Can users with existing sessions **continue working** during an IdP outage?
- [ ] Is **break-glass** in a different failure domain, tested, alerted and documented offline?
- [ ] Are **configuration, keys and certificates** included in backups — and has a full restore been tested?
- [ ] Is identity **first in the organisation's restoration order**?
- [ ] Have you load-tested at **peak**, including the post-outage thundering herd?
- [ ] Are there **synthetic login transactions** monitoring from real user locations?
- [ ] Do you alert on **business outcomes** (joiners not provisioned) as well as system health?

---

**Next:** [Securing the IAM Platform Itself](06-securing-iam.md) →

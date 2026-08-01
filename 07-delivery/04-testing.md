---
title: Testing
parent: 7. Delivery & Operations
nav_order: 4
---

# Testing IAM

## Why identity testing is unusual

Three properties make it harder than typical application testing:

1. **The blast radius of a bug is the whole organisation.** A provisioning defect can grant 5,000 people access they shouldn't have — or remove access from 5,000 people who need it — in one run.
2. **Correctness is often about what *didn't* happen.** "The leaver's access was removed" and "nobody else's was" are both requirements, and the second is far harder to test.
3. **The data is the test.** Identity logic is driven by real-world data shapes — the person with two managers, the contractor with no end date, the name with an apostrophe. Synthetic test data hides exactly the cases that will break production.

{: .concept }
> **Test with production-shaped data, not production data.** You need the *shapes* — the duplicate names, the missing attributes, the 40-character surnames, the person who is both employee and contractor — because those are what break the logic. You must not need the *contents*, because copying real HR records into a test environment is a data protection problem. Pseudonymise or synthesise, but **preserve the distribution and the anomalies**, including the ugly ones.

---

## The test layers

| Layer | Tests | Notes |
|:--|:--|:--|
| **Unit** | Rules, transformations, correlation logic, policy expressions | Where policy is code, test it like code |
| **Integration** | Each connector against a real non-production target | The layer most often skipped for lack of a target |
| **Functional** | Use cases end to end | Directly from the [use cases](03-requirements.md) |
| **Data-driven** | The same flow across many identity shapes | **The highest-value layer in IAM** |
| **Negative** | Access is *not* granted where it shouldn't be | Consistently under-tested |
| **Performance** | Bulk events, peak authentication, campaign load | Bulk is the one that surprises people |
| **Failure** | Target down, credential expired, feed missing, partial failure | Where products genuinely differ |
| **Security** | Privilege escalation, token handling, injection, tenant isolation | See [security review](#security-testing) |
| **Regression** | The full suite, before every release and vendor upgrade | Non-negotiable for SaaS platforms |
| **UAT** | Business users, real scenarios | Managers doing an actual certification |
| **DR** | Restore and failover | Annually at minimum |

---

## Data-driven testing

The layer that catches the most real defects. Build a test population that deliberately includes the awkward shapes:

| Shape | What it catches |
|:--|:--|
| Employee, contractor, intern, partner | Type-specific logic |
| No manager / manager is a leaver | Approval routing failures |
| Two managers, or a matrix reporting line | Model assumptions |
| Missing cost centre or department | Data quality gates |
| Names with apostrophes, hyphens, accents, non-Latin script | Encoding bugs, and the account that can't be created |
| Very long names, very short names | Field truncation |
| Duplicate name, different person | **Correlation false positives — the dangerous one** |
| Same person, two identities | Correlation false negatives |
| Rehire | Reactivation logic |
| Contractor→employee conversion | Persona handling |
| Future-dated start; back-dated termination | Date handling |
| Someone in 40 groups | Token size, PAC limits |
| An identity with no accounts; an account with no identity | Orphan handling |
| Termination recorded *before* the start date | The nonsense case that appears in real HR data |

Run the full lifecycle across all of them, every release. Automating this is one of the highest-return investments in an IAM programme.

---

## Negative testing

The requirement is not only "Maria gets access" but "**only** Maria gets access."

- After a leaver runs, verify that **nobody else's access changed**. Snapshot before and after; diff.
- After a role change, verify the blast radius matches expectation — this is where a mis-scoped dynamic rule shows up.
- Verify that a revoked user genuinely **cannot authenticate**, and that existing sessions and tokens are handled per design.
- Verify SoD **blocks** — the request that should fail.
- Verify tenant isolation in multi-tenant designs by attempting cross-tenant access with a modified identifier.

{: .warning }
> **Snapshot-and-diff should be standard practice for every bulk operation, in test and in production.** Before a bulk change: export the full assignment state. After: export again, diff, and confirm the delta matches expectation. This catches over-broad rules, misconfigured scopes and unintended cascades *before* they become the incident where 4,000 people lost access on a Monday morning. It takes minutes and it has saved many programmes.

---

## Failure testing

Deliberately break things, because these are the paths that determine operability:

- Take a target system offline mid-provisioning. What happens to the queue, and to the user's reported state?
- Expire a connector's credential. Is the failure visible, or silent?
- Send a malformed HR record. Is it quarantined, or does it poison the batch?
- Stop the HR feed entirely. **Does anything alert?** (Test this one — silence looks like "no changes".)
- Return a rate-limit error from a SaaS target. Does backoff work?
- Deliver duplicate and out-of-order events. Is processing idempotent and order-tolerant?
- Fail halfway through a five-system joiner. What's the user's state, who is told, what retries?

---

## Security testing

Beyond functional correctness:

- **Token handling** — signature validation, algorithm confusion, audience validation, expiry ([tokens](../02-identity-fundamentals/06-tokens-and-jwt.md)).
- **Federation** — assertion replay, XML signature wrapping, unscoped assertions from a partner.
- **Injection** — LDAP filter injection, SQL, and anywhere user input reaches a query.
- **Authorisation** — object-level checks, privilege escalation via role manipulation, delegated admin boundary escape.
- **Session** — fixation, cookie attributes, logout effectiveness.
- **Self-service abuse** — enumeration, recovery flow bypass, rate limits.
- **The IAM platform's own attack surface** — admin interfaces, API authentication, ([securing IAM](../04-architecture-practice/06-securing-iam.md)).

Include the identity platform in penetration testing scope. It is frequently excluded on the grounds that it's "infrastructure", which is precisely backwards.

---

## Environments

The perennial constraint. You need at least development, test/QA and production; ideally a staging environment mirroring production configuration.

**The recurring blocker: connected applications that have no non-production instance.** Options, in order of preference: use the vendor's sandbox; build a mock target that speaks the same API; test in production against a small, controlled test account population with monitoring; or accept the risk explicitly and document it. **Identify these during [discovery](02-discovery.md)** — finding them during build is a schedule shock, and finding them at go-live is worse.

---

## Testing at go-live and after

**Go-live:** run a small pilot cohort first; keep the old process available for a defined period; snapshot before cutover; have a rehearsed rollback; monitor the exception queue hourly for the first week.

**After:** the test suite doesn't retire. SaaS platforms upgrade on the vendor's schedule, target APIs change without warning, and connectors break silently. A regression suite that runs on a schedule — not only before releases — is what turns "the connector broke three weeks ago" into "the connector broke last night and we knew by 07:00."

---

## Architect's checklist

- [ ] Does the test population include the **awkward identity shapes**, including the ugly ones?
- [ ] Is the full lifecycle run **data-driven** across that population every release?
- [ ] Is **negative testing** in place — verifying that nothing *else* changed?
- [ ] Is **snapshot-and-diff** standard for bulk operations, in production too?
- [ ] Are **failure scenarios** tested, including a feed that simply stops?
- [ ] Are **bulk events** load-tested at realistic volume?
- [ ] Is the identity platform **in scope for penetration testing**?
- [ ] Have applications with **no non-production environment** been identified and mitigated?
- [ ] Is there a **regression suite that runs on a schedule**, not just at release?
- [ ] Is there a rehearsed **rollback** for go-live?

---

**Next:** [Run & Operations](05-run-and-operations.md) →

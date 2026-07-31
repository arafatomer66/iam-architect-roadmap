---
title: Self-Assessment & Skills Matrix
parent: Start Here
nav_order: 4
---

# Self-Assessment & Skills Matrix

Score yourself honestly. The point is not the number — it's finding the two competencies where you're weakest, because those, not your strongest, determine whether you get architect roles.

## Scoring

For each statement, rate 0–3:

| Score | Meaning |
|:--:|:--|
| **0** | I don't know what this means |
| **1** | I know the term and roughly what it does |
| **2** | I can explain it accurately and use it in a design |
| **3** | I can design with it, teach it, and describe how it fails in production |

Max score per competency: 18. Max total: 180.

---

## 1. IT & infrastructure fundamentals

- [ ] I can explain what a directory service is, and why LDAP looks the way it does
- [ ] I can read an LDAP filter, describe a DN/RDN, and explain schema, objectClass and attribute syntax
- [ ] I can explain AD forests, domains, trusts, sites, GPOs and replication latency
- [ ] I can explain how DNS is used in Kerberos and AD service location (SRV records)
- [ ] I can explain symmetric vs asymmetric crypto, hashing vs encryption, signing vs encrypting, and why password hashing needs a work factor
- [ ] I can explain a certificate chain, revocation (CRL/OCSP), and what actually happens in a TLS handshake

→ Gaps here: [Stage 1](../01-it-fundamentals/)

## 2. Authentication & federation protocols

- [ ] I can draw the SAML SP-initiated flow from memory, including where the assertion is signed and what's validated
- [ ] I can explain the OAuth 2.0 roles, the authorization code flow with PKCE, and why implicit flow was deprecated
- [ ] I can explain the difference between OAuth and OIDC without saying "OIDC is OAuth for authentication" and stopping there
- [ ] I can list what must be validated in a JWT, and name three ways JWT validation is commonly broken
- [ ] I can explain Kerberos: TGT, service ticket, KDC, SPN, and two ways it is attacked
- [ ] I can explain FIDO2/WebAuthn, why it is phishing-resistant, and what a passkey actually is

→ Gaps here: [Stage 2, pages 1–11](../02-identity-fundamentals/)

## 3. Authorisation models

- [ ] I can explain RBAC, ABAC, ReBAC and PBAC and when each is the wrong choice
- [ ] I can explain role explosion, and three ways to avoid it
- [ ] I can explain the difference between a business role and a technical entitlement bundle
- [ ] I can describe policy decision/enforcement/information/administration points (PDP/PEP/PIP/PAP)
- [ ] I can explain externalised authorisation and when it's worth the latency
- [ ] I can model "a manager can approve expenses for their own reports up to €10k, in their own cost centre" in at least two models

→ Gaps here: [Authorisation models](../02-identity-fundamentals/12-authorization-models.md), [Policy as code](../02-identity-fundamentals/13-policy-as-code.md)

## 4. Identity lifecycle & governance

- [ ] I can design a joiner-mover-leaver process including the awkward cases (rehire, contractor→employee, internal transfer with overlap, long leave, death)
- [ ] I can explain birthright access, and its risks
- [ ] I can explain reconciliation and why it matters more than provisioning
- [ ] I can design an access certification campaign that produces genuine decisions rather than rubber-stamping
- [ ] I can explain segregation of duties, toxic combinations, preventive vs detective controls and mitigating controls
- [ ] I can explain role mining, both top-down and bottom-up, and why the output is never directly usable

→ Gaps here: [Stage 2, pages 14–21](../02-identity-fundamentals/)

## 5. Identity data & integration

- [ ] I can define an authoritative source, and resolve a conflict between two of them
- [ ] I can explain correlation, join rules, and what to do with unmatched accounts
- [ ] I can design for eventual consistency: retries, idempotency, ordering, partial failure
- [ ] I can explain SCIM, its limits, and when to use event-driven provisioning instead
- [ ] I can design an integration with a system that has no API
- [ ] I can explain what happens to in-flight lifecycle events when a target system is down for eight hours

→ Gaps here: [Data modelling](../01-it-fundamentals/08-data-modeling.md), [Integration patterns](../04-architecture-practice/03-integration-patterns.md)

## 6. Privileged & non-human identity

- [ ] I can explain vaulting, rotation, session brokering, session recording and JIT elevation
- [ ] I can design break-glass access, including how it's detected and reviewed
- [ ] I can explain the risks of service accounts and how to bring them under governance
- [ ] I can explain workload identity federation and how it removes long-lived secrets
- [ ] I can explain how secrets get into a container without being baked into the image
- [ ] I can describe how an AI agent should hold and use delegated authority

→ Gaps here: [PAM](../02-identity-fundamentals/18-privileged-access-management.md), [NHI](../03-identity-domains/04-non-human-identities.md), [Workload identity](../03-identity-domains/05-workload-identity.md)

## 7. Architecture practice

- [ ] I can produce a target-state architecture *and* a sequenced path to it
- [ ] I can write an ADR that someone disagreeing with me would still call fair
- [ ] I can design an IAM estate that tolerates the loss of one region
- [ ] I can explain the availability requirement difference between AM and IGA, and design accordingly
- [ ] I can run a coexistence period where two IdPs are live simultaneously
- [ ] I can name five IAM anti-patterns and why each is tempting

→ Gaps here: [Stage 4](../04-architecture-practice/)

## 8. Business, risk & compliance

- [ ] I can build a business case for an IAM programme with credible numbers
- [ ] I can name the identity-relevant controls in SOX, ISO 27001, PCI DSS and NIST 800-53
- [ ] I can explain what evidence an auditor wants for access review, and produce it
- [ ] I can explain how GDPR/consent affects a CIAM design
- [ ] I can quantify an identity risk in a way a risk committee accepts
- [ ] I can explain a design trade-off to a CFO without using the word "protocol"

→ Gaps here: [Stage 6](../06-business-and-risk/)

## 9. Delivery & operations

- [ ] I can scope an IAM discovery and know what artefacts to demand
- [ ] I can write requirements that survive a vendor RFP without being vendor-shaped
- [ ] I can design the operating model: who runs this after go-live, with what SLAs
- [ ] I can plan testing for an IGA programme, including data-driven and negative tests
- [ ] I can write a runbook a 3am on-call engineer can follow
- [ ] I can lead an identity incident: compromised admin account, right now

→ Gaps here: [Stage 7](../07-delivery/)

## 10. The frontier

- [ ] I can explain Zero Trust in terms of identity without repeating marketing
- [ ] I can explain continuous access evaluation / shared signals (CAEP, SSF) and why session revocation is hard
- [ ] I can explain ITDR and what identity attack paths look like
- [ ] I can explain verifiable credentials, DIDs, and where they realistically apply
- [ ] I can describe the identity model for autonomous AI agents acting on users' behalf
- [ ] I can explain crypto agility and what post-quantum means for an identity estate

→ Gaps here: [Stage 8](../08-frontier/)

---

## Reading your score

| Total | Where you are | What to do |
|:--|:--|:--|
| **0–45** | Newcomer | Follow [Path A](03-how-to-use-this-repo.md) start to finish. Don't shortcut Stage 1 |
| **46–90** | Practitioner | You can operate. Fill Stage 2 holes, then go hard at Stages 4 and 6 |
| **91–130** | Senior engineer / emerging architect | Your gap is almost certainly competencies 7, 8 and 9 — not more protocol depth |
| **131–160** | Architect | Push your weakest two to 3s; start writing and teaching. Depth in [Stage 8](../08-frontier/) is your differentiator |
| **161–180** | Principal | Either you're being modest, or you should be contributing to this repo |

{: .architect }
> **The asymmetry that matters:** two competencies at 3 and eight at 1 makes you a specialist engineer. All ten at 2 makes you an architect. Breadth with sufficient depth beats narrow mastery in this role — because your job is deciding *between* areas, and you can't arbitrate a domain you can't reason about.

---

## Re-take schedule

Score yourself now, then again at **6 and 12 months**. Track the shape of the radar, not the total. The healthy trajectory is the low bars rising, not the high bars getting higher.

Record it however you like; a table in a notebook is enough:

| Competency | Now | +6 mo | +12 mo |
|:--|:--:|:--:|:--:|
| 1. IT fundamentals | | | |
| 2. AuthN & federation | | | |
| 3. Authorisation | | | |
| 4. Lifecycle & governance | | | |
| 5. Data & integration | | | |
| 6. Privileged & non-human | | | |
| 7. Architecture practice | | | |
| 8. Business & risk | | | |
| 9. Delivery & operations | | | |
| 10. Frontier | | | |

---

**Next:** [The 12-Month Learning Plan](05-learning-plan.md) →

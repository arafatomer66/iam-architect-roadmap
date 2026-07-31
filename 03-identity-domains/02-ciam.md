---
title: Customer Identity (CIAM)
parent: 3. Identity Domains
nav_order: 2
---

# Customer Identity & Access Management

## A different discipline that shares a vocabulary

CIAM uses the same protocols as workforce identity and almost none of the same assumptions.

| | Workforce | CIAM |
|:--|:--|:--|
| Users | Thousands | Millions to hundreds of millions |
| Created by | HR | **The user, unsupervised** |
| Can you mandate MFA? | Yes | Only by accepting abandonment |
| Cost of friction | Complaints | **Lost revenue, immediately measurable** |
| Identity data | Given by the employer | Given by the user, subject to consent |
| Support | Internal helpdesk | Must be self-service — you cannot staff for it |
| Availability | Business hours matter most | **Peak traffic events; downtime is public** |
| Attack profile | Targeted, insider | Automated, at scale: bots, stuffing, fraud |
| Governing constraint | Audit | **Privacy law and conversion rate** |

{: .concept }
> **In CIAM, the identity system is part of the product.** Registration is the first experience a customer has of your company, and login is the tollgate on every subsequent one. A workforce IdP that adds three seconds and one extra click is an annoyance; a CIAM flow that does the same measurably reduces revenue. This changes what "good" means: the design goal is **the least identity friction consistent with the risk of what the user is doing.**

---

## The distinctive requirements

### Scale and availability

Millions of accounts, unpredictable peaks (a marketing campaign, a product launch, Black Friday, a breaking news event), and global distribution. Design implications: horizontal scale, read replicas, aggressive caching of what can be cached, multi-region, and load testing at *peak* rather than average. Login is on the critical revenue path — its availability target is the same as the storefront's.

### Progressive profiling

Don't ask for everything at registration. Collect the minimum to create the account, then gather more at the point where it's genuinely needed and the user can see why. Every field on a registration form costs completions, and unused data is a liability under privacy law.

### Consent and privacy

CIAM is where identity meets **GDPR, CCPA/CPRA** and their equivalents:

- **Lawful basis** for each processing purpose, recorded.
- **Consent** that is granular, freely given, withdrawable as easily as given, and **versioned** — you must be able to show what a user agreed to, and when.
- **Data subject rights**: access, rectification, erasure, portability, objection — with an operational process and an SLA, not a mailbox.
- **Data minimisation and retention.** Delete what you no longer need; "we might use it someday" is not a basis.
- **Residency**, where regulation or contract requires data to stay in a region.

{: .architect }
> **Consent is identity data with a lifecycle of its own, and it needs a proper model.** Not a boolean on the user record: a versioned record of *which* purpose, *which* version of the notice, *when*, *through what interface*, and *whether it has since been withdrawn*. Retrofitting this after two years of collection is painful and sometimes impossible — you cannot reconstruct what someone agreed to in 2024 if you only stored `marketing_optin = true`. Design it in at the start; it costs almost nothing then.

### Fraud and abuse

Your registration and login endpoints are public and will be attacked continuously:

| Threat | Mitigation |
|:--|:--|
| **Credential stuffing** | Breached-password screening, rate limiting, device fingerprinting, bot detection, risk-based step-up |
| **Fake account creation** | Email/phone verification, velocity limits, behavioural signals, proofing at value moments |
| **Account takeover** | Anomaly detection, step-up on sensitive actions, notification of profile changes to the **old** contact details |
| **Account enumeration** | Uniform responses and timing for known and unknown accounts |
| **Scraping / API abuse** | Rate limits, authentication on data endpoints, bot management |
| **Synthetic identity fraud** | Proofing, cross-checks, velocity analysis across accounts |

Note the small detail with outsized value: **when someone changes their email or phone, notify the old address too.** It's the cheapest account-takeover detection there is.

### Self-service everything

You cannot staff a helpdesk for ten million users. Registration, recovery, profile management, consent management, MFA enrolment and deletion must all work unaided — and recovery must be **secure enough** without being staffed. This is the hardest single design problem in CIAM, and it is where most account takeovers succeed.

---

## Architectural patterns

### Separate the CIAM estate from workforce

Different scale, different availability profile, different data classification, different regulatory regime, different release cadence. Running customers on the workforce IdP couples your product's availability to your internal identity system and mixes employee and customer data in one blast radius.

**Two systems, one set of standards** is the usual right answer. Share the protocols, the security bar and the architectural principles; separate the instances, the data and the operational teams.

### Social and federated login

Reduces registration friction and removes password management. Costs: dependency on a third party, a subset of users who won't use it, account-linking complexity, and the question of what happens when the social account is deleted or the provider changes its terms.

**Always support account linking** — the same human will arrive via Google today and email/password tomorrow, and creating two accounts is a support burden and a data-quality problem. Link on **verified email** only; linking on unverified email is an account takeover vector.

### Passkeys in CIAM

CIAM is where passkeys deliver most: they remove password reset volume (typically the largest support cost), they defeat credential stuffing outright, and they're *faster* to use, so adoption is achievable without coercion. Offer them, promote them at the right moments, and keep a fallback — but design the fallback so it isn't the weak link ([recovery](../02-identity-fundamentals/08-mfa-and-passwordless.md)).

### B2B2C

Your customer is an organisation whose users you serve — a bank's corporate customers, an insurer's brokers, a platform's merchant accounts. This is a genuine third pattern: consumer-scale, but with organisational structure, delegated administration and per-tenant policy. See [B2B identity](03-b2b-identity.md); trying to force it into a pure CIAM model is a common and expensive mistake.

---

## Metrics that matter

| Metric | Why |
|:--|:--|
| **Registration completion rate** | Direct revenue impact; the number the business already watches |
| **Login success rate** | Failures are lost sessions and support calls |
| **Time to first successful login** | Onboarding quality |
| Password reset volume | The largest support cost; passkeys should visibly reduce it |
| MFA / passkey adoption rate | Security posture without coercion |
| Account takeover rate | The security outcome that matters |
| Auth p95/p99 latency and availability | It's on the revenue path |
| DSR (data subject request) turnaround | Regulatory obligation |

{: .warning }
> **CIAM security decisions are business decisions and must be made with the business.** "Require MFA for all customers" may cut fraud losses and cut revenue by more. The security team cannot make that call alone, and neither can product. Bring **numbers**: fraud loss avoided versus conversion impact, measured, ideally A/B tested. An architect who can run that conversation with data is far more effective than one asserting best practice.

{: .vendor }
> **In the products.** CIAM has its own vendor landscape. **Ping Identity** (PingOne for Customers, PingAM/PingDirectory from the ForgeRock line) and **Okta Customer Identity Cloud** (Auth0) are the enterprise leaders; **Microsoft Entra External ID** (succeeding Azure AD B2C) covers Microsoft-centric estates; **Keycloak** is a credible open-source base if you're prepared to own the operations. **SailPoint and One Identity Manager are not CIAM platforms** — they govern workforce access, and proposing them for a consumer estate is a category error worth catching early in a design review. Evaluate CIAM specifically on: scale and latency under peak, journey/orchestration flexibility, consent and privacy features, bot/fraud integration, and how much of the login UI you can control.

---

## Architect's checklist

- [ ] Is CIAM **architecturally separate** from workforce identity, with shared standards?
- [ ] What is the **availability and latency target** for login, and does it match the revenue-path reality?
- [ ] Is registration **progressive**, collecting the minimum up front?
- [ ] Is **consent modelled properly** — purpose, version, timestamp, channel, withdrawal?
- [ ] Is there an operational process and SLA for **data subject requests**?
- [ ] What defends registration and login against **bots, stuffing and enumeration**?
- [ ] Is **account recovery** secure enough to be unstaffed, and is it your weakest path?
- [ ] Are profile-change notifications sent to the **previous** contact details?
- [ ] Is **account linking** supported, and does it require verified email?
- [ ] Are **passkeys** offered, and is the fallback path as strong?
- [ ] Are security changes justified with **conversion and fraud numbers**, not assertion?
- [ ] If this is really **B2B2C**, is the design acknowledging that?

---

**Next:** [B2B & Partner Identity](03-b2b-identity.md) →

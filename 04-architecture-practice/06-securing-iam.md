---
title: Securing the IAM Platform Itself
parent: 4. Architecture Practice
nav_order: 6
---

# Securing the IAM Platform Itself

## The crown jewel problem

You have spent a career arguing that identity should be centralised. You succeeded. Now **one system authenticates everyone to everything** — which means one system, compromised, hands an attacker everything.

{: .concept }
> **Centralising identity is correct and it concentrates risk.** The trade is worth making: distributed credential handling across 300 applications is far worse in aggregate. But the trade obliges you to protect the centre at a completely different standard from anything it protects. **The IdP is Tier 0.** If it isn't treated that way — separate administration, separate credentials, separate monitoring, separate change control — you have taken the concentration risk without buying the compensating control.

Real-world incidents at identity providers have shown the pattern repeatedly: the initial compromise is often mundane (a support system, a contractor's laptop, a stolen session token), and the consequence is extraordinary because of what the compromised system could reach.

---

## The threat model

| Threat | Path | Consequence |
|:--|:--|:--|
| **Administrative account compromise** | Phishing, session theft, weak recovery | Total: create identities, change policy, disable logging |
| **Signing key theft** | Access to key material, HSM misuse | **Forge assertions for anyone, to anything** — and it is nearly undetectable |
| **Policy tampering** | Legitimate admin access misused, or config drift | Silent removal of MFA requirements; a backdoor conditional access exclusion |
| **Backdoor federation** | Adding a rogue trusted IdP or an extra signing certificate | Attacker asserts any identity; extremely hard to spot without config monitoring |
| **Rogue application registration** | OAuth client with broad application permissions | Credential-less persistence surviving password resets |
| **Directory ACL abuse** | Shadow admin paths through nested rights | Escalation to full control via a path nobody documented |
| **Supply chain** | Vendor compromise, malicious update, support system breach | Depends on the vendor's own security — outside your control |
| **Insider** | Legitimate access misused | Hardest to detect; needs SoD and independent monitoring |
| **Log tampering** | Admin disables or edits audit | Removes your ability to investigate anything else |

{: .warning }
> **Two changes deserve their own alerting, separate from everything else: a new federated trust, and a new signing certificate or key.** Both are legitimate operations performed rarely and deliberately. Both are also exactly what an attacker with administrative access does to establish durable, quiet persistence — a second signing key lets them mint valid assertions indefinitely, and it survives password resets, MFA changes and most incident response. If you build only two detections in your identity estate, build these.

---

## Controls, in priority order

### 1. Administrative access

- **Separate privileged identities.** IdP administration from a dedicated account, never the one that reads email.
- **Phishing-resistant MFA, mandatory, no exceptions** for identity administrators.
- **Privileged access workstations** for Tier 0 administration.
- **Just-in-time elevation** — nobody holds standing Global Admin / super-admin. Eligibility yes; standing access no.
- **Minimum count.** Two to four permanent global administrators, named, reviewed monthly. Everyone else elevates.
- **Break-glass**, protected differently, alerted loudly ([HA & DR](05-ha-dr-and-scale.md)).

### 2. Key material

- Signing keys in an **HSM or KMS** where they cannot be exported.
- **Separate keys per environment**; a dev key that works in production is a production compromise.
- **Rotation rehearsed**, with overlap ([PKI](../01-it-fundamentals/05-pki-and-tls.md)).
- **Alert on any key or certificate addition**, as above.
- Documented emergency rotation: how fast, and what breaks.

### 3. Configuration integrity

- **Config as code** where the product supports it: versioned, reviewed, deployed through a pipeline.
- **Drift detection** — compare running configuration against the intended baseline, continuously.
- **Change control with four eyes** for policy affecting authentication or authorisation.
- **Staged rollout and simulation** before applying policy broadly. The what-if tools exist; use them every time.

### 4. Logging and detection

- Identity logs shipped **out of the identity platform**, to a store administrators of that platform cannot alter. This is what makes investigation possible after an administrative compromise.
- Retention long enough for a realistic dwell time — a year is a common floor.
- Detections for: new federation trust, new signing key, new privileged role assignment, MFA method removal or downgrade, conditional access policy change or exclusion added, mass export or directory read, consent grant of high-privilege application permissions, logging configuration change, service principal credential added.
- See [ITDR](../08-frontier/02-itdr.md).

### 5. Network and platform

- Administrative interfaces restricted by network origin and device compliance where feasible.
- The identity platform's own infrastructure patched on a tighter schedule than everything else.
- For SaaS IdPs: understand and configure the vendor's tenant-hardening options, and monitor the vendor's own security notifications.

### 6. Segregation of duties inside IAM

- The person who **grants** access should not be the person who **approves** it.
- Whoever administers the identity platform should not be the sole reviewer of its logs.
- Provisioning changes should be reviewable by someone outside the IAM team.
- The IAM team's own access should be certified by someone else. **This is routinely missed** — the team that runs access reviews for everyone else is often the one population nobody reviews.

---

## Supply chain and vendor risk

If your IdP is SaaS, its security is largely someone else's operational practice. What you can actually do:

- **Diligence on the vendor's own identity practices** — how do *their* support staff access *your* tenant, and is it brokered, approved and recorded? Ask this specifically; it has been the entry point in real incidents.
- **Contractual breach notification** with a defined timeframe.
- **Understand the blast radius**: what can the vendor's support tooling see and do in your tenant, and can you limit it?
- **Tenant-level hardening** you control: administrative roles, conditional access, session controls, logging export.
- **An exit plan.** If you had to leave in 90 days, could you? Not because you expect to — because knowing the answer changes your negotiating position and reveals your coupling.

---

## Recovery planning

Assume compromise of the identity platform and plan the recovery, because improvising it is not possible:

1. **Contain** — disable suspicious administrative accounts, revoke sessions and tokens estate-wide.
2. **Rotate** — signing keys, administrative credentials, service principal secrets, `krbtgt` (twice, with replication between).
3. **Audit the configuration** — every federation trust, every signing certificate, every privileged role assignment, every conditional access exclusion, every application permission grant, compared against a known-good baseline. **This step is why you keep a baseline.**
4. **Re-establish trust** — relying parties may need to accept new keys, which is an outage you must coordinate.
5. **Re-verify identities** where credential theft is suspected — potentially mass re-enrolment.

{: .architect }
> **The question that reveals whether an identity estate is genuinely defensible: "if an attacker had administrative access to the IdP for a week and then left, how would you know, and how would you know they hadn't left anything behind?"** Answering it requires a configuration baseline, independent immutable logs, and specific detections for persistence mechanisms. Most organisations cannot answer it. Being able to is a meaningful differentiator, and building toward it is a concrete, fundable piece of architecture work.

---

## Architect's checklist

- [ ] Is the identity platform explicitly designated and treated as **Tier 0**?
- [ ] Do identity administrators use **separate accounts, phishing-resistant MFA and JIT elevation**?
- [ ] How many **standing** global administrators exist? Are they reviewed monthly?
- [ ] Are signing keys **non-exportable**, environment-separated, and rotation-rehearsed?
- [ ] Do you alert specifically on **new federation trusts** and **new signing keys**?
- [ ] Is configuration **versioned**, with drift detection against a baseline?
- [ ] Are identity logs shipped to a store **identity admins cannot alter**?
- [ ] Is the **IAM team's own access** certified by someone outside the team?
- [ ] For SaaS: how does the **vendor's support** access your tenant, and is it recorded?
- [ ] Is there a **rehearsed recovery plan** for IdP compromise, including key rotation and config audit?
- [ ] Could you detect a week-old administrative compromise **after the fact**?

---

**Next:** [Anti-Patterns](07-anti-patterns.md) →

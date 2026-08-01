---
title: Compliance & Regulation
parent: 6. Business & Risk
nav_order: 3
---

# Compliance & Regulation

*General educational overview, not legal or compliance advice. Requirements vary by jurisdiction, sector and interpretation — work with your compliance and legal functions.*

## Why architects must know this

Compliance is the most common **trigger** for IAM funding and the most common **constraint** on IAM design. An architect who can read a control requirement and translate it into an architectural obligation is far more effective than one who waits to be told.

{: .concept }
> **Almost every framework asks the same five things about identity**, in different vocabularies: (1) only authorised people have access, (2) access matches the role and is least-privilege, (3) access is removed promptly when no longer needed, (4) privileged access is controlled and monitored, (5) **you can prove all of the above for a past period**. Learn those five and any new framework becomes a mapping exercise rather than a new subject.

---

## The frameworks, and what they want from identity

### SOX (Sarbanes-Oxley) — US-listed companies

Concerned with the integrity of financial reporting, so it targets systems that affect the financial statements.

**Identity obligations:** access to financial systems restricted and approved; **segregation of duties** enforced (the classic create-vendor/approve-payment conflicts); periodic access reviews with evidence; privileged and emergency access controlled and reviewed; change management around access.

**What auditors actually ask for:** a complete user listing for in-scope systems at a point in time, evidence of approval for each access, evidence that reviews happened and that revocations were executed, and a population completeness argument — *how do you know this list is complete?*

### ISO/IEC 27001 (and 27002)

An information security management system standard. The identity-relevant controls in the current 27002 structure cover identity management, access rights, authentication information, privileged access, and information access restriction.

**Character:** ISO cares about the *management system* — that you defined a policy, implemented it, monitored it and improved it. Evidence of process operation matters as much as the control itself.

### PCI DSS — payment card data

The most prescriptive of the major frameworks, and useful for that reason:

- Unique ID for every user with access to cardholder data (**no shared accounts** in scope).
- MFA for all access into the cardholder data environment, and for all remote access.
- Access on a need-to-know, least-privilege basis, formally approved.
- Remove access promptly on termination.
- Review privileged access periodically.
- Strong requirements on vendor/default credentials.

Prescriptive requirements are helpful architecturally — they translate directly into design constraints.

### GDPR / UK GDPR — personal data

Not an access-control framework, but it shapes identity design profoundly:

- **Lawful basis and purpose limitation** for processing identity attributes.
- **Data minimisation** — including which attributes you release in assertions.
- **Data subject rights** — access, rectification, erasure, portability, objection — needing an operational process ([CIAM](../03-identity-domains/02-ciam.md)).
- **Security of processing** — access control is explicitly named.
- **Records of processing**, and **DPIAs** for high-risk processing (biometrics, profiling, monitoring).
- **Employee monitoring** limits — directly relevant to session recording in [PAM](../02-identity-fundamentals/18-privileged-access-management.md), and in several European countries requiring works council consultation.

### HIPAA — US healthcare

Unique user identification, emergency access procedure (**a named requirement for break-glass**), automatic logoff, access authorisation and modification procedures, and audit controls. Notable for making break-glass an explicit obligation rather than a good practice.

### NIS2 (EU) and DORA (EU financial services)

Both raise the bar and both put **management accountability** on the record — which changes the funding conversation, because a named executive is now personally responsible.

Identity-relevant themes: strong access control and MFA; **third-party/ICT supply chain risk management** (which makes [B2B and vendor access](../03-identity-domains/03-b2b-identity.md) a compliance matter, not just a security one); incident reporting within tight deadlines; and, in DORA, testing and resilience requirements that reach identity infrastructure.

### NIST frameworks (US, widely used globally)

- **SP 800-53** — the control catalogue; the AC (Access Control) and IA (Identification and Authentication) families are your material.
- **SP 800-63** — digital identity guidelines: IAL/AAL/FAL ([authentication concepts](../02-identity-fundamentals/01-authentication-concepts.md)). The best available vocabulary for assurance, useful even where it isn't mandated.
- **SP 800-207** — Zero Trust architecture.
- **Cybersecurity Framework (CSF)** — the Identify/Protect/Detect/Respond/Recover structure many boards now use.

### Sector and regional additions

SWIFT CSP (banking), FFIEC (US banking), TISAX (automotive), CMMC (US defence supply chain), eIDAS (EU trust services and identity), PSD2 SCA (EU payments), plus national data-protection and sector regulators. **Find out which apply to your organisation, and read the identity sections yourself** rather than relying on a summary.

---

## Mapping controls to architecture

The practical artefact: a matrix of **requirement → control → mechanism → evidence → owner**.

| Requirement (source) | Control | Mechanism | Evidence | Owner |
|:--|:--|:--|:--|:--|
| Access removed on termination (SOX, PCI, ISO) | Automated leaver processing | HR event → IGA → disable + revoke | Per-user revocation report with timestamps | IAM ops |
| SoD enforced (SOX) | Preventive + detective SoD | Policy check at request; periodic scan | Violation report; mitigation evidence | Finance + IAM |
| Periodic access review (all) | Certification campaigns | Risk-scoped campaigns | Campaign completion + revocation execution | App owners |
| MFA for privileged (PCI, NIS2, DORA) | Phishing-resistant MFA | IdP policy | Policy config + exception register | Security |
| Unique user ID (PCI) | No shared accounts | Vaulting/elimination | Shared account register with status | IAM |
| Emergency access (HIPAA) | Break-glass procedure | Documented, alerted, tested | Usage log + test records | IAM + security |

{: .architect }
> **Build this matrix once and maintain it, and audits stop being projects.** The reason audit preparation consumes person-weeks is that nobody has written down which mechanism satisfies which requirement or where the evidence lives — so it's rediscovered every cycle by whoever is available. The matrix is a few days of work and it pays back at every audit, every new regulation, and every time someone asks "are we compliant with X?" It is also, incidentally, an excellent way to discover requirements nothing currently satisfies.

---

## Evidence: what auditors actually want

1. **Control design** — what the control is, documented, approved.
2. **Control operation** — proof it ran throughout the period, not just today.
3. **Population completeness** — that the list you sampled from is *all* of it. **This is where organisations most often fail**, because they can produce a user list but not demonstrate it's complete.
4. **Sample testing** — pick 25 leavers, show revocation evidence for each.
5. **Exception handling** — what failed, and what happened next. Exceptions handled well demonstrate a working control; exceptions hidden destroy trust in everything else.

Practical guidance: **generate evidence continuously, not at audit time.** If producing it requires a person to assemble spreadsheets, it will be late, inconsistent and expensive — and evidence assembled after the fact is inherently less credible than evidence produced as a by-product of the process.

---

## Compliance ≠ security

The trap in both directions:

- **Compliant but insecure** — every control passes, and MFA excludes 400 exempted executives, service accounts are ungoverned, and NHIs aren't in scope because no framework named them.
- **Secure but non-compliant** — genuinely good controls, no evidence they operate, so the audit fails anyway.

Use compliance as the **floor and the funding lever**, not the ceiling. The strongest position: design for security, then demonstrate compliance as a consequence. When a requirement is genuinely misaligned with security (rules that force periodic password rotation, for instance), document the compensating control and have the conversation with your compliance function rather than quietly ignoring it.

---

## Architect's checklist

- [ ] Which frameworks and regulators **actually apply** to your organisation?
- [ ] Have you read the **identity-relevant sections** yourself?
- [ ] Does a **requirement → control → mechanism → evidence → owner** matrix exist and stay current?
- [ ] Is evidence **generated continuously**, or assembled at audit time?
- [ ] Can you demonstrate **population completeness** for in-scope systems?
- [ ] Are **open findings** tracked with owners and dates, and visible to the roadmap?
- [ ] Are **exceptions** documented and handled, not hidden?
- [ ] Does the design address risks **no framework names** — NHIs, cloud entitlements, session theft?
- [ ] For EU/UK: has **works council / employee monitoring** consultation been considered for PAM and analytics?

---

**Next:** [Zero Trust as a Business Programme](04-zero-trust-business.md) →

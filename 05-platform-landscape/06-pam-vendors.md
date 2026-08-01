---
title: PAM Vendors
parent: 5. Platform Landscape
nav_order: 6
---

# PAM Vendors

*Verify current details against vendor documentation.*

The [PAM concepts](../02-identity-fundamentals/18-privileged-access-management.md) matter more than the products, but you should know the field and — more importantly — the **three distinct problems** that get bundled under "PAM", because vendors are strong in different ones.

| Problem | What it needs |
|:--|:--|
| **1. Human privileged access** | Vaulting, brokered sessions, recording, JIT elevation |
| **2. Endpoint privilege** | Removing standing local admin; per-application elevation |
| **3. Application/machine secrets** | Programmatic secret retrieval, dynamic credentials, rotation |

An organisation typically needs all three and rarely gets them best-of-breed from one vendor. Being explicit about which problem you're solving prevents a great deal of confused procurement.

---

## CyberArk

The market reference, and the most complete portfolio: the Digital Vault (the hardened credential store), Privileged Session Manager (brokered, recorded sessions), Endpoint Privilege Manager, Secrets Manager (including the Conjur lineage), and cloud/identity-security extensions.

**Strengths:** depth and maturity in the classic vault-and-session domain; extensive platform support including mainframe, network devices and industrial systems; regulatory credibility — auditors know it; the largest body of implementation experience in the market.

**Where it's hard:** a substantial deployment footprint and real implementation effort; historically architected around on-premises infrastructure, though the cloud portfolio has grown considerably; cost; and — as with all PAM — the **organisational** resistance, which is the real project risk regardless of vendor.

**Fits:** large regulated enterprises; organisations where an auditor or regulator expects a recognised PAM platform; heterogeneous estates including legacy and OT.

---

## Delinea

Formed from Thycotic and Centrify. Secret Server (vaulting), Privilege Manager (endpoint), Server PAM/Privilege Control (the Centrify identity-consolidation lineage, strong on Linux/Unix AD integration) and cloud offerings.

**Strengths:** frequently faster time-to-value than heavier platforms; strong on Unix/Linux privilege and AD bridging; a good fit for organisations that need solid coverage without a multi-year programme.

**Fits:** mid-market and enterprises wanting quicker deployment; estates with significant Linux/Unix privilege management needs.

---

## BeyondTrust

Password Safe (vaulting), Privilege Management for Windows/Mac/Unix (endpoint), and Remote Support / Privileged Remote Access — the last being a genuine differentiator for the **third-party and vendor remote access** problem.

**Strengths:** endpoint privilege management is a particular strength; the remote-access products address a real and common gap (how do equipment vendors and MSPs get in safely?); solid analytics.

**Fits:** organisations where third-party remote access or endpoint least-privilege is the primary driver.

---

## One Identity Safeguard

Vaulting (Safeguard for Privileged Passwords), session management (Safeguard for Privileged Sessions) and analytics.

**Strengths:** the integration story with [One Identity Manager](02-one-identity.md) — governance of *eligibility* in the IGA platform, control of *exercise* in Safeguard, from one vendor. For estates already committed to One Identity, that coherence is a real advantage. The session-management component (from Balabit's lineage) is well regarded, particularly for proxy-based session recording that doesn't require agents.

**Fits:** existing One Identity customers; organisations valuing the IGA/PAM integration over best-of-breed in each.

---

## HashiCorp Vault

A different animal, and frequently misplaced in comparisons: **Vault is application secrets management, not human PAM.**

**Strengths:** dynamic secrets (generate a short-lived database credential per request — the best available pattern), a rich auth-method model consuming [workload identity](../03-identity-domains/05-workload-identity.md) attestation, encryption-as-a-service, PKI issuance, and deep developer adoption.

**Weaknesses as human PAM:** no session brokering or recording, no endpoint privilege management, and its interfaces are developer-oriented rather than aimed at an operations team.

**The common architecture:** a traditional PAM platform for humans and infrastructure, Vault for applications and platform automation. That's two products doing two different jobs, and it's usually correct rather than duplicative — but the boundary should be stated explicitly, or both teams will assume the other has a case covered.

---

## Cloud-native privileged access

Increasingly displacing the *cloud portion* of traditional PAM:

- **Entra PIM** — JIT activation of directory and Azure roles, with approval and time bounds.
- **AWS IAM Identity Center** with session policies and short-lived role sessions.
- **GCP privileged access management** and short-lived credentials.
- **Kubernetes** RBAC with short-lived tokens and impersonation.

These are native, well-integrated and free of an agent footprint. The question is not "vault or native?" but **"where is the boundary, and who governs eligibility across both?"** A common, defensible answer: native JIT for cloud control planes, vault for servers, databases, network devices and legacy — with IGA governing eligibility for everything so there's one place to certify who *may* become privileged.

---

## Selecting

Weight these, in roughly this order:

1. **Coverage of your actual estate.** Test the awkward targets — mainframe, network devices, OT, legacy databases, your specific cloud — not the demo ones.
2. **The three problems.** Which do you actually need, and does this vendor do all of them, or are you buying two products?
3. **Operational model.** What happens when the vault is unavailable at 3am? Is there a designed break-glass path?
4. **Time to value.** A phased rollout starting with Tier 0 should show results within a quarter.
5. **Adoption friction.** Administrators will resist. Which product adds least friction for the workflows they use every day? This determines whether the deployment succeeds, and it is consistently underweighted in evaluations.
6. **Session recording and privacy.** In Europe especially, works council consultation and employee-monitoring law are real constraints. Involve legal before you select, not before you deploy.
7. **Integration with IGA** — can eligibility be governed and certified in your IGA platform?

{: .architect }
> **PAM projects fail on adoption, not on technology.** Every product in this list can vault a credential and record a session. The one that succeeds in your organisation is the one your administrators will actually use when they're under pressure at 2am with a production incident — because the alternative is that they keep a copy of the password "just in case", and your coverage metric becomes fiction. **Run the evaluation with your most sceptical senior administrator in the room, and take their objections seriously.** They are describing your adoption risk.

---

**Next:** [Choosing a Platform](07-choosing-a-platform.md) →

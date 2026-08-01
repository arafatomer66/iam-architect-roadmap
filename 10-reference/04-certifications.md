---
title: Certifications
parent: 10. Reference
nav_order: 4
---

# Certifications

## The honest position

{: .concept }
> **Certifications are a hiring filter, not a learning method.** They get your CV past a screen and satisfy a partner-status requirement. They do not make you an architect, and studying for one before you understand the underlying concepts produces exam-shaped knowledge that evaporates. **Do the learning first, take the exam late** — in month 10–12 of the [plan](../00-start-here/05-learning-plan.md), when it costs you two weeks instead of two months.

The exception: if a job advert lists a certification as mandatory and you want that job, get it. That's a rational, transactional reason, and it's fine.

*Certification names, structures and prerequisites change. Verify current details with the issuing body.*

---

## Vendor-neutral

### IDPro CIDPRO

The identity profession's own certification, based on the **IDPro Body of Knowledge**. Vendor-neutral, concept-focused, and covers exactly the ground this repo does: protocols, lifecycle, governance, architecture.

**Worth it if:** you want a credential that signals *identity* expertise rather than *product* expertise. Increasingly recognised, and the Body of Knowledge is genuinely worth reading regardless of whether you sit the exam.

### CISSP (ISC²)

Broad security certification. Domain 5 is Identity and Access Management, but it's one of eight — you'll spend most of the effort on other domains.

**Worth it if:** you're moving into security leadership, or job adverts in your market demand it (common in the US and in government-adjacent roles). Requires five years of experience.

### CISM / CRISC (ISACA)

Management and risk oriented. **CISM** for security management, **CRISC** for risk. Both useful for the [business and risk](../06-business-and-risk/) half of the architect role, and both well recognised by audit and risk functions — which matters more than technical audiences realise.

### CCSP, cloud security certifications

Relevant if your track is cloud/workload identity.

### TOGAF

Enterprise architecture methodology. Divisive: valuable in organisations that use it, and irrelevant elsewhere. **Check whether your target employers care** before investing.

---

## Cloud platform

| Certification | Relevance to identity |
|:--|:--|
| **Microsoft SC-300** (Identity and Access Administrator) | **The most directly relevant cloud identity certification.** Entra ID, Conditional Access, PIM, governance. High value if you're in a Microsoft estate |
| **Microsoft SC-100** (Cybersecurity Architect) | Broader; useful for the architect framing |
| **AWS Security Specialty** | IAM policies, federation, KMS, workload identity |
| **Google Professional Cloud Security Engineer** | GCP IAM, workload identity federation |
| **Azure AZ-500** | Broader Azure security |

Cloud certifications age faster than most; the platforms change quarterly.

---

## Vendor product

| Vendor | Typical structure |
|:--|:--|
| **SailPoint** | Certified engineer/consultant tracks for IdentityIQ and Identity Security Cloud. Often required for partner-firm consultants |
| **One Identity** | Certification programmes for Identity Manager, Active Roles and Safeguard |
| **Ping Identity** | Certifications across PingFederate, PingOne and the PingAM/DS line |
| **Okta** | Certified Professional / Administrator / Consultant / Developer / Architect ladder — one of the better-structured vendor programmes |
| **CyberArk** | Defender / Sentry / Guardian ladder for PAM |
| **Saviynt** | Product certifications, growing |

**Get a vendor certification when:** you work with that product daily; your employer is a partner and needs certified staff; or a specific role demands it.

**Don't get one when:** you're trying to break into IAM and think it substitutes for fundamentals. It doesn't, and interviewers can tell within two questions.

---

## A sensible order

**If you're new to IAM:**
1. Learn the concepts first — [Stages 1–2](../02-identity-fundamentals/).
2. **SC-300** or the cloud certification matching your estate. Practical, well-scoped, immediately useful.
3. **CIDPRO** once you have breadth — it validates the vendor-neutral depth this repo teaches.
4. A vendor certification for whichever product you actually use.
5. **CISSP or CISM** later, if you're moving toward security leadership.

**If you're already an engineer:**
1. Whichever vendor certification your daily product offers — it's a short step from where you are.
2. **CIDPRO** to demonstrate the conceptual breadth that distinguishes architects.
3. **CISM or CRISC** to strengthen the risk and business half.

---

## What actually gets you hired as an architect

In roughly descending order of weight:

1. **Demonstrable design experience.** Architectures you designed, decisions you made, outcomes you can describe.
2. **Depth in an interview.** The [whiteboard](../09-practice/03-whiteboard-scenarios.md) is where the decision is made.
3. **Breadth across the domain** — being able to reason about governance *and* protocols *and* business.
4. **Communication** — evident in the interview itself.
5. **A referral.**
6. **Certifications.**

{: .architect }
> **If you have limited time, write and speak rather than certify.** A published article explaining federation clearly, a conference talk, an internal design document you can describe, or a public repository demonstrates the thing certifications only proxy for. It also compounds — people find it, cite it, and bring you opportunities. A certification is a one-time signal that expires; a body of explanatory work is a standing one.

Which is, incidentally, an argument for building something like this repo yourself.

---

**Next:** [Templates](05-templates.md) →

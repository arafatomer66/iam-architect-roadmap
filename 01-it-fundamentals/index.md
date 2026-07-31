---
title: 1. IT Fundamentals
nav_order: 2
has_children: true
---

# Stage 1 — IT Fundamentals

> *"Strong IT foundation is the base of IAM expertise."*

## Why this stage is not optional

Identity is a **distributed systems problem wearing a security costume**. Every genuinely hard IAM incident resolves to a fundamentals problem:

| The symptom | The actual cause |
|:--|:--|
| "SSO stopped working for everyone at 09:00" | Signing certificate expired |
| "Kerberos authentication fails intermittently" | Clock skew > 5 minutes on one DC |
| "The user was added to the group but still can't access the share" | Replication latency, or the Kerberos ticket predates the change |
| "Provisioning created 4,000 duplicate accounts overnight" | A non-idempotent connector retried after a timeout |
| "Federation works from the office but not from home" | Split-horizon DNS returning the internal IdP address |
| "The app can't validate our tokens" | JWKS endpoint unreachable, or key rotation without cache invalidation |
| "Access reviews show entitlements that don't exist" | Reconciliation never ran against the target's real state |

Notice that **none** of these are identity-protocol problems. They are DNS, certificates, time, replication, retry semantics and caching. An architect who cannot reason about that layer will design systems that work in the demo and fail in production.

## Pages in this stage

| # | Page | Core question it answers |
|:--|:--|:--|
| 1 | [Directory Services & LDAP](01-directory-services.md) | Why is identity data stored in trees, and how do you query them? |
| 2 | [Active Directory Deep Dive](02-active-directory.md) | How does the directory 90% of enterprises still run actually work? |
| 3 | [Networking & DNS for IAM](03-networking-and-dns.md) | How do clients find identity services, and why does DNS break SSO? |
| 4 | [Cryptography for IAM](04-cryptography.md) | What guarantees do signatures, hashes and encryption actually give you? |
| 5 | [PKI, Certificates & TLS](05-pki-and-tls.md) | What does a certificate assert, and why do certs cause so many outages? |
| 6 | [Windows & Linux Administration](06-os-administration.md) | How do operating systems represent identity locally? |
| 7 | [Cloud Fundamentals](07-cloud-fundamentals.md) | How do AWS, Azure and GCP model identity and permissions? |
| 8 | [Data Modelling & Integration](08-data-modeling.md) | How do you keep identity data correct across dozens of systems? |
| 9 | [HTTP, APIs & Webhooks](09-http-and-apis.md) | What is actually happening on the wire during every IAM flow? |

{: .architect }
> **Depth calibration for this stage:** you need to *reason correctly and troubleshoot*, not to be a specialist. You will never be the best network engineer in the room — but when the network engineer says "DNS is fine", you must know what question to ask next.

## The through-line

Everything in Stage 1 exists to support one claim you will make thousands of times in your career:

> *"This request came from this subject, over this channel, and I can prove both."*

Directories store the subject. Crypto and PKI protect the proof. DNS and networking establish the channel. Cloud and OS layers enforce the result. Data modelling keeps the subject accurate over time.

---

**Start:** [Directory Services & LDAP](01-directory-services.md) →

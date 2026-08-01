---
title: Post-Quantum & Crypto Agility
parent: 8. The Frontier
nav_order: 5
---

# Post-Quantum & Crypto Agility

## The assumption that broke

Public-key cryptography rests on problems classical computers cannot solve efficiently: factoring large integers (RSA) and the discrete logarithm (Diffie-Hellman, ECDSA, EdDSA).

**Shor's algorithm solves both efficiently on a sufficiently large quantum computer.** Such a machine does not exist today. Whether and when it will is genuinely uncertain, and anyone giving you a confident date — in either direction — is overselling.

{: .concept }
> **"Harvest now, decrypt later" is why this matters before the machine exists.** An adversary can record encrypted traffic today and decrypt it whenever they acquire the capability. So the question is not *"when will quantum computers arrive?"* but **"how long must today's data stay confidential?"** If the answer is fifteen years — health records, state secrets, long-term contracts, biometric templates — then the exposure is already real. For a session token with a five-minute lifetime, it is not.

Symmetric cryptography is much less affected: Grover's algorithm gives a quadratic speed-up, addressed by doubling key sizes. **AES-256 is considered fine. Hashes are fine with adequate output length.** The problem is specifically **public-key** cryptography — which is exactly what identity systems use for signatures, key exchange and certificates.

---

## Where identity is exposed

| Use | Algorithm | Exposure |
|:--|:--|:--|
| **TLS key exchange** | ECDH | **Highest** — harvest-now-decrypt-later applies directly to recorded sessions |
| **TLS server certificates** | RSA/ECDSA signatures | Forgery risk, but only *after* a capable machine exists — short cert lifetimes limit this |
| **Token and assertion signing** | RS256, ES256, EdDSA | Signature forgery, post-capability. Short token lifetimes limit exposure |
| **SAML assertion signing** | RSA/ECDSA | Same |
| **FIDO2 / passkeys** | ECDSA / EdDSA | Credential forgery, post-capability |
| **Smart cards / PIV** | RSA/ECC | Long-lived certificates are more exposed |
| **Code signing** | RSA/ECDSA | Long verification lifetimes — a real concern |
| **Device identity in OT/IoT** | RSA/ECC, often burned in | **Structurally hardest** — 20-year lifespans, no ability to update ([OT/IoT](../03-identity-domains/06-ot-and-iot.md)) |
| **Encrypted data at rest** | AES | Low — symmetric, and adequately sized |

Note the pattern: **short-lived artefacts are relatively safe; long-lived ones are the problem.** That's a useful triage heuristic and it maps directly onto action.

---

## The standards

NIST completed its post-quantum standardisation and published the first algorithms in 2024:

| Standard | Algorithm | Purpose |
|:--|:--|:--|
| **FIPS 203 (ML-KEM)** | CRYSTALS-Kyber | Key encapsulation — TLS key exchange |
| **FIPS 204 (ML-DSA)** | CRYSTALS-Dilithium | Digital signatures — the general-purpose choice |
| **FIPS 205 (SLH-DSA)** | SPHINCS+ | Hash-based signatures — conservative, larger, useful where confidence matters more than size |

Practical characteristics that affect *your* designs: **keys and signatures are substantially larger** than their elliptic-curve equivalents. That matters concretely for identity — larger signatures in JWTs pushing against header size limits, larger certificates in TLS handshakes, and constrained devices that may not have the memory. **Hybrid modes** (classical + post-quantum together, so the result is secure if either holds) are the standard transitional approach and are already deployed in TLS by major browsers and cloud providers.

---

## Crypto agility: the actual deliverable

You cannot predict which algorithm will be needed or when. You *can* build systems that change algorithms without re-architecture — and that capability pays off for every future cryptographic transition, not just this one.

**What crypto agility means concretely:**

| Property | Test |
|:--|:--|
| **Algorithms are configuration, not code** | Can you change signing algorithm without a code release? |
| **Key and signature sizes aren't assumed** | Are there fixed-size buffers, database columns or header limits that would break with larger keys? |
| **Multiple algorithms can run simultaneously** | Can you publish two signing keys with different algorithms during a transition? |
| **The verifier decides what it accepts** | Never the token ([tokens](../02-identity-fundamentals/06-tokens-and-jwt.md)) |
| **You have a complete cryptographic inventory** | Do you know every place crypto is used, with what algorithm, key size and expiry? |
| **Rotation is rehearsed** | Can you actually roll a signing key without an outage? ([PKI](../01-it-fundamentals/05-pki-and-tls.md)) |
| **Dependencies are known** | Which vendors, libraries and devices constrain your choices? |

{: .architect }
> **Crypto agility is the deliverable; post-quantum is one reason to want it.** An organisation that can change its signature algorithm in a quarter is prepared for post-quantum, for the next algorithm deprecation (SHA-1 and RSA-1024 both required exactly this), and for a surprise vulnerability. An organisation that cannot is facing a multi-year project regardless of which cryptographic change arrives. **Make agility the goal in your design reviews, and post-quantum becomes a scheduled migration rather than an emergency.**

---

## A proportionate plan

**Now — inventory and agility:**

1. Build a **cryptographic inventory**: every certificate, key, algorithm and its expiry, across the identity estate. Most organisations don't have one, and it's needed for [certificate management](../01-it-fundamentals/05-pki-and-tls.md) anyway.
2. Identify **long-lived exposures** — data requiring 10+ years of confidentiality, device certificates with long lifetimes, code signing.
3. Fix agility blockers: hardcoded algorithms, fixed-size assumptions, single-key-only designs.
4. **Ask vendors for their roadmaps.** IdP, HSM/KMS, PKI, device manufacturers. Their timelines constrain yours, and asking early moves their priorities.
5. Shorten certificate and token lifetimes where practical — this reduces exposure to *every* cryptographic risk, not only this one.

**Next — hybrid where it's cheap:**

6. Enable **hybrid key exchange in TLS** where your stack supports it. Widely available already, and it directly addresses harvest-now-decrypt-later.
7. Test post-quantum algorithms in non-production to find the size-related breakages before they matter.

**Later — full migration:**

8. Migrate signatures and certificates as vendor and library support matures.
9. Plan the **OT/IoT fleet** separately and early — it is the hardest and slowest, and devices deployed today will still be running when this matters.

---

## Proportionality

This topic attracts both dismissal and alarmism. The defensible position:

- **Not urgent for short-lived artefacts.** A five-minute access token signed with ES256 is not a meaningful post-quantum risk.
- **Genuinely relevant now for long-confidentiality data** and for **long-lived device credentials**.
- **Crypto agility is worth doing regardless**, on its own merits.
- **Regulated sectors will get told.** Financial and government sectors should expect requirements with dates; watch your regulator rather than the vendor marketing.

Being the architect who has an inventory and an agility assessment when that requirement arrives — rather than starting from zero — is the entire practical point of engaging with this now.

---

## Architect's checklist

- [ ] Is there a **cryptographic inventory** of the identity estate — algorithms, key sizes, expiries, owners?
- [ ] Which data or credentials require **10+ years** of protection?
- [ ] Are algorithms **configuration rather than code**?
- [ ] Would **larger keys and signatures** break anything — buffers, columns, header limits, constrained devices?
- [ ] Can you run **two signing algorithms simultaneously** during a transition?
- [ ] Is **key rotation rehearsed** and overlap-based?
- [ ] Have you asked your **IdP, HSM, PKI and device vendors** for post-quantum roadmaps?
- [ ] Is **hybrid TLS key exchange** enabled where supported?
- [ ] Is there a separate, early plan for **OT/IoT device credentials**?
- [ ] Could you change your signature algorithm in **one quarter** if required?

---

**Next:** [Stage 9 — Practice](../09-practice/) →

---
title: MFA & Passwordless
parent: 2. Identity Fundamentals
nav_order: 8
---

# MFA & Passwordless

## Not all MFA is equal

"We have MFA" is one of the least informative statements in security. The methods differ by orders of magnitude in the attacks they stop.

| Method | Stops password reuse | Stops real-time phishing | Stops device malware | Notes |
|:--|:--:|:--:|:--:|:--|
| SMS / voice OTP | ✅ | ❌ | ❌ | SIM swap, SS7, and the code is phishable. NIST discourages it |
| Email OTP | ✅ | ❌ | ❌ | As strong as the email account — often the same account being protected |
| TOTP app (Google/Microsoft Authenticator) | ✅ | ❌ | ⚠️ | Better; still typed into whatever page asked |
| Push notification (approve/deny) | ✅ | ❌ | ⚠️ | **MFA fatigue**: spam until someone taps approve |
| **Push + number matching** | ✅ | ⚠️ | ⚠️ | Defeats blind fatigue attacks; still relay-able in principle |
| Mobile app with device binding | ✅ | ⚠️ | ⚠️ | Depends on implementation |
| **FIDO2 / WebAuthn / passkey** | ✅ | ✅ | ✅ | Origin-bound; the phishing site can't use it |
| **Smart card / PIV / certificate** | ✅ | ✅ | ✅ | Hardware-backed; heavy to operate |

{: .concept }
> **Phishing resistance is the dividing line, and it's binary.** Any method where the user *transcribes or approves a code* can be relayed by an adversary-in-the-middle proxy: the user hands the code to a fake site, which forwards it to the real one in real time. Toolkits automating this are commodity. Only **cryptographic origin binding** — where the authenticator refuses to sign for `micros0ft-login.com` because it isn't the origin it was registered to — actually stops it. That's FIDO2/WebAuthn and certificate-based authentication. Everything else buys time, not immunity.

---

## The attacks your design must survive

| Attack | How | Response |
|:--|:--|:--|
| **AiTM phishing** | Proxy site relays credentials and OTP live, steals the resulting **session cookie** | Phishing-resistant factors; **token binding**; detect impossible sessions; short sessions |
| **MFA fatigue / push bombing** | Repeated prompts until someone approves | Number matching, rate limits, alert on repeated denials |
| **SIM swap** | Attacker ports the number | Eliminate SMS for anything valuable |
| **OTP interception** | Malware, SS7, notification preview on lock screen | Suppress content in notifications; move off OTP |
| **Session/token theft** | Infostealer takes cookies after successful MFA | **This bypasses MFA entirely** — needs device binding, sender-constrained tokens, continuous evaluation |
| **Helpdesk social engineering** | Attacker calls to reset MFA | Strong recovery process — see below |
| **Registration attack** | Attacker enrols their own factor on an account with none | Enforce enrolment at first login, from a trusted context |

{: .warning }
> **Session theft is the reason MFA alone is no longer sufficient.** Infostealer malware harvests authenticated session cookies from browsers; the attacker imports them and is inside — having never touched the credential or the second factor. The architectural answers are device-bound sessions, sender-constrained tokens ([DPoP/mTLS](06-tokens-and-jwt.md)), shorter session lifetimes for sensitive applications, and continuous evaluation ([CAEP](../08-frontier/01-zero-trust.md)). If your MFA rollout treated login as the finish line, this is the gap.

---

## Passwordless

Passwordless removes the password entirely rather than adding to it. Two very different things get called it:

1. **Genuinely passwordless** — a passkey, smart card, or platform authenticator is the *primary* credential. There is no password to phish, reuse or leak.
2. **Password-hidden** — a password still exists in the backend (magic links, some mobile flows). Better UX, similar risk surface.

Aim for the first. And note the ordering that many programmes get wrong:

{: .architect }
> **Passwordless before MFA-everywhere is usually the better sequence** for the *high-value* population. Adding a second factor to a password keeps the password — and the password remains phishable, resettable by a helpdesk, and stuffable. Replacing it with a passkey removes an entire attack class *and* improves the user experience, which is why adoption succeeds. The pragmatic path: passkeys for administrators and high-risk roles first, then broad workforce rollout, with password+MFA as the fallback tier — not the destination.

The hard part is never the primary credential. It's **the residue**: legacy protocols that only accept passwords, service accounts, break-glass, shared kiosks, offline scenarios, and account recovery. Going *fully* passwordless means eliminating all of those, and that's a multi-year programme.

---

## Recovery: the weakest link

Your effective authentication strength is the **weakest path to an authenticated session**, and that is almost always recovery.

| Recovery method | Strength | Notes |
|:--|:--|:--|
| Second registered authenticator | **Strong** | The right default: enrol two from day one |
| In-person / video identity verification | Strong | Costly; right for administrators and high-value customers |
| Temporary Access Pass issued by a verified process | Medium–strong | Time-boxed, single-use, audited |
| Manager attestation | Medium | Workable in workforce contexts, with logging |
| Emailed reset link | Weak | Only as strong as the mailbox |
| Security questions | **Very weak** | Answers are public or forgotten. Retire them |
| Helpdesk call with knowledge questions | **Very weak** | The path attackers actually use — including in several high-profile breaches |

Design rule: **recovery must be at least as strong as the credential it replaces**, and every recovery event must be logged, alerted on, and reviewable. If a helpdesk agent can reset a phishing-resistant factor after three easily-researched questions, you don't have phishing-resistant authentication — you have a phishing-resistant *front door* next to an open window.

---

## Rolling out MFA without a revolt

Sequence that works:

1. **Administrators and privileged roles first** — phishing-resistant only, no exceptions. Smallest population, highest risk.
2. **External access** — VPN, remote, anything internet-facing.
3. **High-value applications** — finance, HR, source control, cloud consoles.
4. **Everyone, everywhere** — including internal access. "Inside the network" is not a trust signal.
5. **Close the legacy gaps** — this is where the real work is: basic-auth protocols, service accounts, unmanaged devices, exceptions.

Practices that reduce pain: enrol **two** factors at registration (removes most recovery calls); allow enrolment during onboarding while identity is already being verified; use **risk-based prompting** so trusted devices aren't challenged constantly; communicate in terms of the user's own risk, not compliance; and staff the helpdesk for the first two weeks.

{: .warning }
> **Exceptions become the attack surface.** Every MFA exemption must have an owner, a business reason, a compensating control and an **expiry date**. Exemption lists that only grow are how "we have MFA" and "the attacker didn't need MFA" coexist in the same incident report. Report the exception count as a KPI.

---

## Standards and mechanisms worth naming

- **TOTP** ([RFC 6238](https://datatracker.ietf.org/doc/html/rfc6238)) — 30-second codes from a shared secret. That shared secret is enrolled once and is stealable at enrolment.
- **HOTP** (RFC 4226) — counter-based; the ancestor.
- **FIDO2 / WebAuthn / CTAP** — public-key authentication with origin binding. See [next page](09-fido2-and-passkeys.md).
- **PIV / CAC / smart cards** — certificate-based, hardware-held, common in government.
- **EMV / dynamic linking** (PSD2 SCA) — payment-specific strong customer authentication, where the challenge is bound to the transaction amount and payee.
- **Certificate-based authentication** — device or user certificates, strong and operationally heavy.

---

## Architect's checklist

- [ ] Which MFA methods are **permitted**, and which are **phishing-resistant**?
- [ ] Are **administrators and privileged roles** on phishing-resistant methods only?
- [ ] Is **SMS** still an option anywhere it matters?
- [ ] Is push protected by **number matching** and rate limits?
- [ ] Is **recovery** as strong as the primary credential, for every population?
- [ ] Are users enrolled with **two** authenticators?
- [ ] How many **exemptions** exist? Does each have an owner, a compensating control and an expiry?
- [ ] What stops **session theft** after successful MFA — device binding, short sessions, sender-constrained tokens, continuous evaluation?
- [ ] Are **legacy authentication protocols** that bypass MFA blocked, and do you have telemetry proving it?
- [ ] What is the plan and sequence toward **passwordless**?

---

**Next:** [FIDO2, WebAuthn & Passkeys](09-fido2-and-passkeys.md) →

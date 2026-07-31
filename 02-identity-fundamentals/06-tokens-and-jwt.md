---
title: Tokens & JWT
parent: 2. Identity Fundamentals
nav_order: 6
---

# Tokens & JWT

## What a token is

A token is **a portable, time-limited artefact that a system accepts in place of re-authenticating the subject.** Session cookies, Kerberos tickets, SAML assertions, JWTs, API keys and refresh tokens are all tokens; they differ in format, lifetime, audience and what proves the bearer is entitled to use them.

{: .concept }
> **The single most important property of a token is whether it is a *bearer* token.** A bearer token grants access to *whoever holds it*, with no further proof — like cash. Steal it, use it. Every token design decision (lifetime, transport, storage, binding) is fundamentally about limiting the consequences of that fact. The alternative — **sender-constrained** tokens, bound to a key the legitimate holder possesses — is where high-assurance systems are heading.

---

## JWT structure

Three base64url segments separated by dots: `header.payload.signature`.

```
eyJhbGciOiJSUzI1NiIsImtpZCI6IjJhZjcxIn0      ← header
.eyJpc3MiOiJodHRwczovL2lkcC5leGFtcGxlLmNvbSI…  ← payload (claims)
.MEUCIQDf8H2n…                                  ← signature
```

**Header** — `alg` (algorithm) and `kid` (which key). **Payload** — the claims. **Signature** — over `base64(header).base64(payload)`.

{: .warning }
> **A JWT is signed, not encrypted.** The payload is base64 — *encoding*, not encryption. Anyone holding the token can read every claim, including the user. Never put anything in a JWT you wouldn't hand to the user: no secrets, no internal hostnames, no data subject to confidentiality obligations. If you genuinely need confidentiality, that's **JWE** (a nested, encrypted JWT) — and most teams that think they need it actually just need to stop putting sensitive data in tokens.

### Registered claims

| Claim | Meaning | Validation |
|:--|:--|:--|
| `iss` | Issuer | Must exactly match expected |
| `sub` | Subject | The stable identifier |
| `aud` | Audience | **Must contain you** |
| `exp` | Expiry | Must be future |
| `nbf` | Not before | Must be past |
| `iat` | Issued at | Sanity + freshness policy |
| `jti` | Token ID | Replay detection / revocation lists |

---

## Validation: the checklist that *is* your security

1. **Parse without trusting.** Never act on claims before the signature verifies.
2. **Pin the algorithm.** The verifier decides the acceptable `alg` set; the token never does.
3. **Resolve the key from a trusted source only** — a configured JWKS URI or static key. Never from `jku`, `x5u`, or an embedded key in the token.
4. **Verify the signature.**
5. **Check `iss`**, exact string match.
6. **Check `aud`** contains this service.
7. **Check `exp` / `nbf`**, with minimal skew (60 seconds is generous).
8. **Check scopes/permissions** for the operation.
9. **Optionally check revocation** — introspection or a `jti` denylist for high-value operations.

### The classic vulnerabilities

| Vulnerability | Mechanism |
|:--|:--|
| **`alg: none`** | Token claims it's unsigned; a permissive library accepts it |
| **Algorithm confusion** | Token changed from `RS256` to `HS256`; the verifier uses the *public* key as an HMAC secret — and public keys are public |
| **`kid` injection** | `kid` used as a file path or SQL value → traversal or injection |
| **`jku` / `x5u` abuse** | Verifier fetches keys from a URL the token supplies |
| **Missing `aud` check** | Token minted for a low-value API accepted by a high-value one |
| **Expiry not checked** | Astonishingly common in internal services |
| **"Decode to get the user ID"** | No validation at all; anyone can mint claims |

All of them reduce to one rule: **the token must never influence how it is validated.**

---

## Lifetime design

| Token | Typical | Driver |
|:--|:--|:--|
| Access token | 5–60 min | Revocation latency vs AS load |
| ID token | 5–60 min | Consumed once at login |
| Refresh token | hours → months | UX vs theft window; rotate + detect reuse |
| SAML assertion | 2–10 min | Consumed immediately; only needs to survive the redirect |
| Session cookie | Idle 15 min – 12 h; absolute 8–24 h | Risk of the application |

{: .architect }
> **Token lifetime is your revocation SLA.** With self-contained JWTs and no introspection, a disabled user keeps working until their access token expires. If your security policy says "access revoked within 5 minutes" and your access tokens live an hour, **your architecture does not meet your policy** — and nobody will notice until an incident. Either shorten lifetimes, introspect on sensitive operations, or adopt continuous evaluation ([CAEP](../08-frontier/01-zero-trust.md)). Make the number explicit in the design and check it against the policy.

---

## Revocation strategies

| Strategy | Revocation latency | Cost |
|:--|:--|:--|
| Short expiry only | Up to token lifetime | None — simplest |
| **Introspection** (opaque or JWT) | Immediate | Network call per request; AS becomes a runtime dependency |
| Denylist by `jti` | Immediate | Shared cache/store on every RS |
| Refresh-token revocation | Up to access-token lifetime | Standard; combine with short access tokens |
| **Continuous evaluation (CAEP)** | Near-immediate | Requires participating RPs; emerging |

Most real designs land on: **short-lived access tokens + refresh token rotation with reuse detection + introspection for high-risk operations.**

---

## Sender-constrained tokens

Bearer tokens are stolen constantly — from logs, browser storage, memory, proxies. Binding a token to a key its legitimate holder controls removes the value of theft.

- **mTLS-bound tokens** ([RFC 8705](https://datatracker.ietf.org/doc/html/rfc8705)) — the token carries a hash of the client certificate; the RS checks the TLS client cert matches. Strong, but needs certificate infrastructure.
- **DPoP** ([RFC 9449](https://datatracker.ietf.org/doc/html/rfc9449)) — the client sends a per-request proof JWT signed with a key bound to the token. Works without mTLS, suits SPAs and mobile.

If you handle payments, health data or privileged administrative APIs, sender-constrained tokens should be on your roadmap. Ask about them in vendor selection — support is uneven.

---

## Other token types you'll design with

| Type | Notes |
|:--|:--|
| **Opaque / reference tokens** | Random string; all meaning at the AS. Best revocability, worst latency |
| **API keys** | Long-lived, often no expiry, frequently in code. Treat as [non-human identities](../03-identity-domains/04-non-human-identities.md) needing owner, rotation and expiry |
| **Personal access tokens (PATs)** | A *user's* long-lived credential used by automation. Governance nightmare: they inherit the user's privilege and survive their departure unless explicitly killed. Enforce expiry, scope, and include them in leaver processing |
| **Session cookies** | The token most applications actually rely on. `HttpOnly`, `Secure`, deliberate `SameSite`, rotate on privilege change |
| **Kerberos tickets** | Binary, offline-verifiable, effectively unrevocable until expiry |
| **SAML assertions** | XML, single-use, very short-lived |
| **Macaroons / biscuits** | Attenuable tokens — a holder can *narrow* capability without contacting the issuer. Niche, elegant, worth knowing exists |

---

## Token exchange and delegation chains

Real systems chain: a user calls service A, which calls B, which calls C. Three options, in increasing correctness:

1. **Forward the user's token.** Simple; every downstream service gets a token with the full original audience and scope. Poor least privilege, and audience validation becomes meaningless.
2. **Client credentials at each hop.** B calls C as itself — the user's identity is lost, so C can't enforce user-level authorisation or produce a useful audit trail.
3. **Token exchange** ([RFC 8693](https://datatracker.ietf.org/doc/html/rfc8693)). B exchanges the incoming token for one scoped to C, preserving `sub` (the user) and recording `act` (the acting party). Both least privilege *and* accountability.

Token exchange is also the right primitive for **AI agents acting on a user's behalf** — the delegation chain is exactly what you need to record and constrain. See [agentic identity](../08-frontier/03-ai-agents.md).

---

## Architect's checklist

- [ ] Is the accepted **algorithm pinned** in every validator, with `none` impossible?
- [ ] Are keys resolved **only** from configured trusted sources?
- [ ] Does every resource server validate **`iss`, `aud`, `exp`** and scope?
- [ ] Does anything in production **decode without validating**?
- [ ] Does your **token lifetime meet your stated revocation SLA**? Have you actually compared the two numbers?
- [ ] Is **refresh token rotation with reuse detection** enabled where public clients exist?
- [ ] Are any **sensitive claims** present in tokens that the user can read?
- [ ] Are **PATs and API keys** inventoried, scoped, expiring, and covered by the leaver process?
- [ ] Do service-to-service chains preserve **user identity via token exchange**, or lose it?
- [ ] Which APIs warrant **sender-constrained** tokens, and is that on the roadmap?

---

**Next:** [SCIM & Provisioning](07-scim.md) →

---
title: Standards & RFC Index
parent: 10. Reference
nav_order: 3
---

# Standards & RFC Index

What each specification says, and when you'd actually reach for it.

{: .architect }
> **Read the source at least once for the four you use daily** — RFC 6749 (OAuth), RFC 7519 (JWT), OIDC Core, and RFC 7644 (SCIM). Not to memorise, but because summaries drop the exact caveats that later cause your production incident. Everything else on this page is lookup material.

---

## OAuth 2.x

| Spec | Title | When you need it |
|:--|:--|:--|
| [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749) | OAuth 2.0 Framework | The core. Roles, grants, endpoints |
| [RFC 6750](https://datatracker.ietf.org/doc/html/rfc6750) | Bearer Token Usage | How tokens travel; why bearer is risky |
| [RFC 7636](https://datatracker.ietf.org/doc/html/rfc7636) | PKCE | Now recommended for all clients |
| [RFC 7009](https://datatracker.ietf.org/doc/html/rfc7009) | Token Revocation | Revocation endpoint |
| [RFC 7662](https://datatracker.ietf.org/doc/html/rfc7662) | Token Introspection | Opaque tokens; immediate revocation |
| [RFC 8628](https://datatracker.ietf.org/doc/html/rfc8628) | Device Authorization Grant | TVs, CLIs, IoT |
| [RFC 8693](https://datatracker.ietf.org/doc/html/rfc8693) | Token Exchange | **Delegation chains, service-to-service, AI agents** |
| [RFC 8705](https://datatracker.ietf.org/doc/html/rfc8705) | mTLS Client Auth & Certificate-Bound Tokens | Sender-constrained tokens |
| [RFC 9068](https://datatracker.ietf.org/doc/html/rfc9068) | JWT Profile for Access Tokens | Interoperable access token format |
| [RFC 9126](https://datatracker.ietf.org/doc/html/rfc9126) | Pushed Authorization Requests | High-assurance; stops parameter tampering |
| [RFC 9396](https://datatracker.ietf.org/doc/html/rfc9396) | Rich Authorization Requests | Fine-grained authorisation detail beyond scopes |
| [RFC 9449](https://datatracker.ietf.org/doc/html/rfc9449) | DPoP | Proof-of-possession without mTLS |
| [RFC 9700](https://datatracker.ietf.org/doc/html/rfc9700) | Security Best Current Practice | **Read this one** — consolidated current guidance |
| OAuth 2.1 (draft) | Consolidation | PKCE required; implicit and ROPC removed |

---

## OpenID Connect

| Spec | Covers |
|:--|:--|
| **OIDC Core** | ID token, flows, claims, validation rules |
| **OIDC Discovery** | `/.well-known/openid-configuration` |
| **OIDC Dynamic Registration** | Programmatic client onboarding |
| **OIDC Session Management / Front-Channel Logout / Back-Channel Logout** | The logout family — read all three before promising SLO |
| **OIDC RP-Initiated Logout** | Logout initiated by the application |
| **CIBA** | Client-Initiated Backchannel Authentication — decoupled/out-of-band approval |
| **FAPI 1.0 / 2.0** | Financial-grade hardened profile. The reference for high assurance |
| **OID4VCI / OID4VP** | Verifiable credential issuance and presentation |

---

## JOSE

| Spec | Covers |
|:--|:--|
| [RFC 7515](https://datatracker.ietf.org/doc/html/rfc7515) | JWS — signatures |
| [RFC 7516](https://datatracker.ietf.org/doc/html/rfc7516) | JWE — encryption |
| [RFC 7517](https://datatracker.ietf.org/doc/html/rfc7517) | JWK — key format, JWKS |
| [RFC 7518](https://datatracker.ietf.org/doc/html/rfc7518) | JWA — algorithms |
| [RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519) | **JWT** |
| [RFC 8725](https://datatracker.ietf.org/doc/html/rfc8725) | **JWT Best Current Practices** — the attack list and mitigations |
| SD-JWT / SD-JWT VC | Selective disclosure JWTs — verifiable credentials |

---

## SAML & legacy federation

| Spec | Covers |
|:--|:--|
| **SAML 2.0 Core** (OASIS) | Assertions and protocols |
| **SAML 2.0 Bindings** | Redirect, POST, Artifact, SOAP |
| **SAML 2.0 Profiles** | Web Browser SSO, SLO |
| **SAML 2.0 Metadata** | Trust establishment |
| **XML Signature / XML Encryption** (W3C) | The crypto layer — and the XSW attack surface |
| **WS-Federation / WS-Trust** | Legacy SOAP-era federation |

---

## Provisioning & events

| Spec | Covers |
|:--|:--|
| [RFC 7642](https://datatracker.ietf.org/doc/html/rfc7642) | SCIM — concepts and use cases |
| [RFC 7643](https://datatracker.ietf.org/doc/html/rfc7643) | SCIM — core schema |
| [RFC 7644](https://datatracker.ietf.org/doc/html/rfc7644) | SCIM — protocol |
| [RFC 8417](https://datatracker.ietf.org/doc/html/rfc8417) | Security Event Token (SET) |
| **SSF / CAEP / RISC** (OpenID Foundation) | Shared signals, continuous access evaluation, account events |

---

## Directory & Kerberos

| Spec | Covers |
|:--|:--|
| [RFC 4510–4519](https://datatracker.ietf.org/doc/html/rfc4510) | LDAP v3 (roadmap, protocol, models, schema, URLs) |
| [RFC 4515](https://datatracker.ietf.org/doc/html/rfc4515) | LDAP filter string representation — **escaping rules** |
| [RFC 2696](https://datatracker.ietf.org/doc/html/rfc2696) | Simple paged results control |
| [RFC 4533](https://datatracker.ietf.org/doc/html/rfc4533) | Content synchronisation |
| [RFC 4120](https://datatracker.ietf.org/doc/html/rfc4120) | Kerberos v5 |
| [RFC 4178](https://datatracker.ietf.org/doc/html/rfc4178) | SPNEGO |

---

## Authentication & credentials

| Spec | Covers |
|:--|:--|
| **W3C WebAuthn (Level 2/3)** | The browser API |
| **FIDO CTAP2** | Platform ↔ authenticator |
| [RFC 4226](https://datatracker.ietf.org/doc/html/rfc4226) / [RFC 6238](https://datatracker.ietf.org/doc/html/rfc6238) | HOTP / TOTP |
| [RFC 8176](https://datatracker.ietf.org/doc/html/rfc8176) | Authentication method reference values (`amr`) |
| **NIST SP 800-63-3** (and revisions) | **Digital identity guidelines — IAL/AAL/FAL.** The most useful single document in identity |

---

## PKI & transport

| Spec | Covers |
|:--|:--|
| [RFC 5280](https://datatracker.ietf.org/doc/html/rfc5280) | X.509 certificates and CRL profile |
| [RFC 6960](https://datatracker.ietf.org/doc/html/rfc6960) | OCSP |
| [RFC 8446](https://datatracker.ietf.org/doc/html/rfc8446) | TLS 1.3 |
| [RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555) | ACME — automated certificate issuance |
| **CA/Browser Forum Baseline Requirements** | What public CAs must do; where certificate lifetimes are decided |
| **FIPS 203 / 204 / 205** | Post-quantum: ML-KEM, ML-DSA, SLH-DSA |

---

## Authorisation

| Spec | Covers |
|:--|:--|
| **XACML 3.0** (OASIS) | Policy language; origin of PDP/PEP vocabulary |
| **ALFA** | Readable syntax compiling to XACML |
| **AuthZEN** (OpenID Foundation) | Emerging standard authorisation API — worth tracking |
| **Cedar** (open source) | AWS's authorisation language |
| **Rego / OPA** | CNCF policy language |
| **Zanzibar paper** (Google) | The ReBAC model behind OpenFGA and SpiceDB |

---

## Identity data & decentralised identity

| Spec | Covers |
|:--|:--|
| **W3C Verifiable Credentials Data Model** | VC structure |
| **W3C Decentralised Identifiers (DIDs)** | DID syntax and resolution |
| **ISO/IEC 18013-5** | Mobile driving licence (mDL) |
| **eIDAS 2.0 / EUDI Wallet ARF** | The EU regulatory and architecture framework |

---

## Control frameworks

| Framework | Identity-relevant parts |
|:--|:--|
| **NIST SP 800-53** | AC (Access Control) and IA (Identification & Authentication) families |
| **NIST SP 800-207** | Zero Trust architecture |
| **NIST CSF** | Identify / Protect / Detect / Respond / Recover |
| **ISO/IEC 27001 & 27002** | Identity, access rights, authentication, privileged access controls |
| **CIS Controls** | Control 5 (account management), Control 6 (access control management) |
| **PCI DSS** | Requirements 7 and 8 — access and authentication |
| **SOC 2** | Common Criteria — logical access |
| **MITRE ATT&CK** | Credential Access, Persistence, Privilege Escalation tactics — read the identity techniques |

---

## How to keep track

- **IETF OAuth working group** and **OpenID Foundation** mailing lists — where the next thing is being argued about before it's a product.
- **NIST** publications and drafts — 800-63 revisions matter to everyone.
- **W3C WebAuthn and VC working groups.**
- Your **vendors' release notes** — a scheduled reading task, not an ad-hoc one.

---

**Next:** [Certifications](04-certifications.md) →

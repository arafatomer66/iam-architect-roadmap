---
title: Glossary
parent: 10. Reference
nav_order: 1
---

# Glossary

250+ terms. Where a term is commonly misused, the misuse is noted.

---

## A

**AAL** — Authenticator Assurance Level (NIST 800-63). AAL1 single factor, AAL2 MFA, AAL3 hardware-based and phishing-resistant.

**ABAC** — Attribute-Based Access Control. Decisions from attributes of subject, resource, action and environment. Expressive; hard to answer "who has access?"

**Access certification** — Periodic review confirming access is still appropriate. Also called attestation or access review.

**Access management (AM)** — The runtime half of IAM: authentication, SSO, MFA, session, federation. Distinct from governance.

**Access request** — A user's request for an entitlement, subject to policy and approval.

**Access token** — An OAuth credential authorising API calls. Audience is the resource server. Should be short-lived. **Not** for the client to inspect.

**Account** — A subject's representation *in one system*. Many accounts per identity. Commonly confused with identity.

**ACE / ACL** — Access Control Entry / List. Permissions attached to a resource.

**ACR** — Authentication Context Class Reference. Claim describing the assurance level of an authentication.

**Adaptive authentication** — Authentication requirements varied by risk signals at runtime.

**AD / AD DS** — Active Directory Domain Services. Microsoft's on-premises directory, Kerberos KDC and policy engine.

**AD CS** — Active Directory Certificate Services. Microsoft's PKI; misconfigured templates (ESC1–ESC8) are a known escalation path.

**AGDLP** — Accounts → Global groups → Domain Local groups → Permissions. Microsoft's group nesting pattern; the ancestor of RBAC's two-layer model.

**Aggregation** — Reading accounts and entitlements from a connected system into an IGA platform.

**Agent (AI)** — Software acting autonomously on a user's behalf. Needs its own identity plus explicit, attenuating delegation.

**alg** — JWT header claim naming the signature algorithm. **The verifier must pin it, never trust it.**

**AMR** — Authentication Methods References. Claim listing the methods used (`pwd`, `otp`, `hwk`). How you know MFA actually happened.

**Anchor** — The immutable join key in a synchronisation relationship. Choosing a mutable one is a long-term defect.

**API key** — A long-lived, often unscoped, frequently unrotated credential. Treat as a non-human identity.

**Assertion** — A signed statement about a subject (SAML).

**Attestation** — (1) Certification of access. (2) In FIDO2, a statement about the authenticator's make and model. (3) In workload identity, the platform vouching for a workload.

**Attribute** — A property of an identity. Has an authoritative source, or should.

**Audience (`aud`)** — Who a token is for. **Must be validated**, or a token for one API is accepted by another.

**Authentication (AuthN)** — Proving control of a credential bound to an identity. Does not prove who the person is.

**Authoritative source** — The system that wins when sources disagree, per attribute.

**Authorisation (AuthZ)** — Deciding what a proven identity may do.

**Authorization code** — Short-lived, single-use OAuth artefact exchanged for tokens over a back channel.

**Authorization server (AS)** — OAuth component issuing tokens.

**azp** — Authorized party. The client a token was issued to, when multiple audiences exist.

---

## B

**B2B identity** — Identity for users belonging to partner organisations.

**B2B2C** — Your customer is an organisation whose users you serve. Consumer scale with organisational structure.

**Bearer token** — A token granting access to whoever holds it. No possession proof. The default, and the reason token theft works.

**Birthright access** — Access granted automatically from attributes. Efficient; the largest source of over-provisioning.

**Break-glass** — Emergency access that works when normal identity systems don't. Must be tested, alerted, rotated.

**Broker** — A component fronting multiple upstream IdPs, normalising identity and policy. The dominant enterprise federation topology.

---

## C

**CAEP** — Continuous Access Evaluation Profile. Standardised events (session revoked, credential changed) pushed between providers.

**Capability** — An unforgeable token conferring specific authority.

**Certificate** — A signed binding of a public key to an identity, valid for a period.

**Certification campaign** — A scheduled access review exercise.

**CIAM** — Customer Identity and Access Management. Consumer scale, self-service, privacy- and conversion-driven.

**CIEM** — Cloud Infrastructure Entitlement Management. Discovering and rightsizing cloud permissions.

**Claim** — A statement about a subject inside a token.

**Client (OAuth)** — The application requesting access. Confidential (can keep a secret) or public (cannot).

**Client credentials** — OAuth flow where a client authenticates as itself. Machine-to-machine.

**Conditional Access** — Microsoft Entra's policy engine: signals → controls.

**Confused deputy** — A privileged component tricked into misusing its authority on another's behalf. Why ID tokens have `aud`; also the core risk in agentic systems.

**Connector** — Integration between an IAM platform and a target system. Code, therefore it decays.

**Consent** — A user's authorisation for a client to act on their behalf, or for data processing. In CIAM, must be versioned and withdrawable.

**Correlation** — Matching accounts to identities. False positives are security incidents.

**Credential** — The secret, key or authenticator proving an identity.

**CRL** — Certificate Revocation List.

**Crypto agility** — The ability to change algorithms without re-architecture.

**CTAP2** — Client to Authenticator Protocol. The FIDO2 half between platform and authenticator.

---

## D

**DACL** — Discretionary Access Control List (Windows). Explicit deny beats allow at the same level.

**DCSync** — Attack abusing directory replication rights to extract password hashes.

**Delegated administration** — Allowing a partner or department to administer its own users, within limits.

**Delegation** — Acting on another's behalf. Kerberos delegation; OAuth token exchange; agent delegation.

**Deprovisioning** — Removing accounts and entitlements. Where audits are failed.

**Device code flow** — OAuth flow for input-constrained devices. Phishable if the approval screen is unclear.

**DID** — Decentralised Identifier. Subject-controlled identifier resolvable to public keys.

**DirSync** — AD control for incremental change retrieval. Basis of most AD sync connectors.

**Directory** — A read-optimised, hierarchical, loosely-consistent, schema-defined store. **Not a database.**

**DIT** — Directory Information Tree.

**DN / RDN** — Distinguished Name / Relative DN. **A location, not an identifier** — it changes when objects move.

**DPoP** — Demonstrating Proof of Possession (RFC 9449). Sender-constrained tokens without mTLS.

**Dynamic role** — Membership calculated from a condition, continuously re-evaluated. Powerful; always simulate before activating.

---

## E

**eIDAS** — EU regulation on electronic identification and trust services. eIDAS 2.0 mandates digital identity wallets.

**Entitlement** — A grantable permission *in a target system*. Defined by the application, never by IAM.

**Entra ID** — Microsoft's cloud identity service. **Not** Active Directory.

**EntityID** — Unique identifier of a SAML IdP or SP. Must match exactly everywhere.

**Ephemeral credential** — Short-lived, issued on demand. The destination for machine authentication.

**EKU** — Extended Key Usage. What a certificate may be used for. A mismatch causes confusing failures.

**Externalised authorisation** — Moving the authorisation decision out of applications into a shared PDP.

---

## F

**FAL** — Federation Assurance Level (NIST 800-63).

**Federation** — Trusting another organisation's authentication. Transfers a risk decision across a boundary.

**FIDO2** — WebAuthn + CTAP2. Phishing-resistant public-key authentication with origin binding.

**FSMO** — Flexible Single Master Operations. Five AD roles that aren't multi-master.

---

## G

**Global Catalog** — Partial forest-wide replica of every AD object.

**gMSA** — Group Managed Service Account. Auto-rotated 240-bit password. The answer to Kerberoastable service accounts.

**Golden ticket** — Forged Kerberos TGT using a stolen `krbtgt` key.

**Governance (IGA)** — Deciding who should have what, proving it, and revoking it.

**Grant** — (1) An OAuth flow type. (2) An assignment of access.

**Group** — A directory construct carrying membership, usually mapped to entitlements.

---

## H

**Harvest now, decrypt later** — Recording encrypted traffic today to decrypt when quantum capability exists.

**HSM** — Hardware Security Module. Keys cannot be exported; signing happens inside.

**Home realm discovery** — Determining which IdP should authenticate a given user.

**Hybrid identity** — On-premises directory synchronised with a cloud identity service. Doubles failure modes, permanently.

---

## I

**IAL** — Identity Assurance Level (NIST 800-63). How well the identity was proofed at enrolment.

**Identity** — The correlated record representing one subject across systems. **Not** an account.

**Identity Cube** — SailPoint's term for the correlated identity.

**IdP** — Identity Provider. Authenticates and issues assertions/tokens.

**ID token** — OIDC JWT describing the authentication event. For the client. **Not** an API credential.

**IGA** — Identity Governance and Administration.

**Impersonation** — Acting *as* another identity, losing the actor's identity. Contrast delegation.

**Introspection** — Asking the authorization server about a token's validity (RFC 7662). Enables immediate revocation.

**ITDR** — Identity Threat Detection and Response. Identity posture + detection + identity-aware response.

**IT Shop** — One Identity Manager's access request catalogue.

---

## J

**JIT (just-in-time)** — (1) Provisioning an account on first login. (2) Elevating privilege for a bounded window.

**JML** — Joiner, Mover, Leaver.

**JOSE** — JSON Object Signing and Encryption. JWS, JWE, JWK, JWA.

**JWE** — JSON Web Encryption.

**JWK / JWKS** — JSON Web Key / Key Set. The `jwks_uri` endpoint verifiers fetch keys from.

**JWS** — JSON Web Signature.

**JWT** — JSON Web Token. **Signed, not encrypted** — the payload is readable by anyone holding it.

---

## K

**KDC** — Key Distribution Center. Kerberos's trusted third party. Every AD domain controller is one.

**Kerberoasting** — Requesting service tickets for SPNs and cracking them offline.

**Kerberos** — Ticket-based authentication protocol. The first widely deployed SSO.

**Key rotation** — Replacing keys. **Must overlap** or federation breaks at the moment of switch.

**kid** — Key ID in a JWT header. Must never be used unvalidated as a path or query value.

**krbtgt** — The AD account whose key signs all TGTs. Rotate twice on compromise.

---

## L

**LAPS** — Local Administrator Password Solution. Unique, rotated local admin passwords per machine.

**LDAP** — Lightweight Directory Access Protocol.

**LDAP injection** — Unescaped input in a filter, turning an authentication check into "match everything".

**Least privilege** — Minimum access needed. Universally endorsed, rarely implemented, requires usage data.

**Lifecycle state** — Pre-hire, active, leave, notice, terminated, archived. Drives birthright and revocation.

**Link** — SailPoint's term for an account correlated to an identity.

---

## M

**mDL** — Mobile driving licence (ISO 18013-5).

**Metadata (SAML)** — XML describing endpoints and certificates. The trust-establishment artefact. Should be URL-based and auto-refreshed.

**Metaverse** — The joined central view in a synchronisation engine.

**MFA** — Multi-factor authentication. Factors from *different* categories. Not all MFA is phishing-resistant.

**MFA fatigue** — Spamming push prompts until someone approves. Countered by number matching.

**Micro-certification** — A small, event-triggered access review. Far more effective than mass campaigns.

**Mitigating control** — A compensating control where an SoD violation can't be resolved. Needs evidence it operates.

**Mover** — A lifecycle change: transfer, promotion, manager change. **Where privilege creep happens.**

**mTLS** — Mutual TLS. Both ends authenticate with certificates.

---

## N

**NameID** — SAML's subject identifier. Format choice matters — persistent and opaque is usually right.

**NHI** — Non-Human Identity. Service accounts, keys, tokens, workloads, agents. Usually 10–100× the human population.

**nonce** — Single-use value preventing replay. Required in OIDC.

**NTLM** — Legacy Windows authentication. Hash *is* the credential; enables pass-the-hash. Should be eliminated.

---

## O

**OAuth 2.0/2.1** — Delegated authorisation framework. **Not an authentication protocol.**

**OCSP** — Online Certificate Status Protocol. Revocation checking; commonly soft-fails.

**OIDC** — OpenID Connect. Identity layer on OAuth 2.0.

**OPA / Rego** — Open Policy Agent and its policy language.

**Orphan account** — An account with no correlated identity. The key reconciliation output.

---

## P

**PAC** — Privilege Attribute Certificate. Group SIDs inside a Kerberos ticket. Size limits constrain group counts.

**PAM** — Privileged Access Management.

**PAP / PDP / PEP / PIP** — Policy Administration / Decision / Enforcement / Information Point.

**PAR** — Pushed Authorization Requests (RFC 9126).

**Pass-the-hash / pass-the-ticket** — Reusing stolen credential material directly.

**Passkey** — A FIDO2 credential, discoverable and often synced. Device-bound vs synced is an enterprise decision.

**Pairwise identifier** — A different subject identifier per relying party. Prevents cross-RP correlation.

**PAT** — Personal Access Token. A user's long-lived credential used by automation. Governance hazard.

**Persona** — One human acting in multiple capacities. Must be modelled explicitly.

**Phishing resistance** — The property that an authenticator refuses to work on the wrong origin. Binary, and the dividing line in MFA.

**PKCE** — Proof Key for Code Exchange (RFC 7636). Binds an authorization code to the client instance. Now recommended for all clients.

**PKI** — Public Key Infrastructure.

**Policy as code** — Authorisation policy in a language, version-controlled, tested, deployed via pipeline.

**Preventive control** — Blocking before it happens. Cheaper than detecting afterwards.

**Privileged** — Access that can bypass controls, access data at scale, change configuration, affect availability, or act as others.

**Provisioning** — Creating or updating accounts and entitlements in a target.

---

## R

**RBAC** — Role-Based Access Control. Certifiable, business-legible, prone to role explosion.

**ReBAC** — Relationship-Based Access Control. Zanzibar-style; access derives from a graph.

**Reconciliation** — Comparing intended state against a target's actual state. **The actual control**; provisioning alone is just automation.

**Refresh token** — Long-lived OAuth token used to obtain new access tokens. Rotate, and detect reuse.

**RelayState** — SAML parameter carrying deep-link context. Must be validated.

**Relying party (RP)** — The application consuming an assertion or token.

**Resource server (RS)** — The API validating access tokens.

**RID** — Relative Identifier. The unique-per-domain part of a Windows SID.

**Risk-based authentication** — See adaptive authentication.

**Role** — A business-meaningful bundle of entitlements. Business roles vs technical/IT roles — keep them separate.

**Role explosion** — Uncontrolled role proliferation, often more roles than users.

**Role mining** — Deriving candidate roles from data. Output is never directly usable.

**ROPC** — Resource Owner Password Credentials. Deprecated; reinstates the password anti-pattern.

---

## S

**SACL** — System Access Control List (Windows). What gets audited. Empty SACL = no events.

**SAML** — Security Assertion Markup Language 2.0. XML-based federation.

**SCIM** — System for Cross-domain Identity Management. REST/JSON provisioning standard. "SCIM-compliant" varies enormously.

**Scope (OAuth)** — Coarse, consent-oriented labels on what a client may attempt. **Not an authorisation model.**

**Sender-constrained token** — Bound to a key the holder possesses (mTLS, DPoP). Theft alone is insufficient.

**Service account** — A non-human account used by an application. Static passwords, over-privilege, no owner.

**SET** — Security Event Token (RFC 8417).

**Session fixation** — Attacker plants a known session ID that survives login. Prevented by regenerating on privilege change.

**SID** — Security Identifier. Windows principals are identified by SID, not name.

**SID history** — Migrated SIDs retained for compatibility. An escalation vector if not cleaned up.

**Silver ticket** — Forged service ticket using a stolen service account key. Generates no KDC logs.

**SLO** — Single Logout. Standardised, and unreliable in practice.

**SoD** — Segregation of Duties. Preventing one person from completing a high-risk transaction alone.

**SP** — Service Provider (SAML). The relying application.

**SPIFFE / SVID** — Platform-neutral workload identity and its credential.

**SPN** — Service Principal Name. Kerberos service identifier. Duplicates break authentication.

**SPNEGO** — Negotiate authentication over HTTP. How Kerberos reaches the browser.

**SSF** — Shared Signals Framework. Delivery of security events between providers.

**SSO** — Single Sign-On.

**Step-up authentication** — Requiring stronger or fresher authentication for a sensitive action. What makes long sessions defensible.

**Strangler fig** — Migration pattern where the new system takes traffic incrementally behind a facade.

**sub** — Subject. The stable identifier in a token. Key accounts on `iss` + `sub`, never on email.

**Synthetic transaction** — An automated end-to-end test running continuously in production. The highest-value identity monitoring.

---

## T

**Target system** — A connected application receiving provisioning.

**TGT / TGS** — Ticket Granting Ticket / Service. Kerberos artefacts.

**Tier model** — Tier 0 identity infrastructure, Tier 1 servers, Tier 2 workstations. Credentials must never cross downward.

**Token exchange** — RFC 8693. Swapping a token for a narrower one, preserving subject and recording actor.

**TOTP** — Time-based One-Time Password (RFC 6238). Better than SMS; still phishable.

**Toxic combination** — A set of entitlements that together enable fraud. See SoD.

---

## U

**UPN** — User Principal Name. `user@domain`, unique per forest. Usually the SSO identifier.

**userAccountControl** — AD bit-flag attribute: disabled, password-never-expires, no-preauth, trusted-for-delegation.

---

## V

**VC / VP** — Verifiable Credential / Presentation. Holder-held, cryptographically signed claims.

**Virtual directory** — A runtime view over multiple backing stores. Excellent transition tool, questionable permanent architecture.

---

## W

**WebAuthn** — The W3C browser API half of FIDO2.

**Workload identity** — Authentication based on platform attestation rather than a stored secret.

**Workload identity federation** — Exchanging a platform-issued token for a cloud credential. Removes stored keys.

---

## X, Z

**XACML** — XML-based authorisation policy standard. Origin of the PDP/PEP vocabulary.

**XSW** — XML Signature Wrapping. SAML attack where the validated element differs from the consumed one.

**Zanzibar** — Google's ReBAC system; the model behind OpenFGA and SpiceDB.

**Zero Trust** — No implicit trust from network location; verify explicitly; least privilege; assume breach.

---

**Next:** [Protocol Cheat Sheets](02-protocol-cheatsheets.md) →

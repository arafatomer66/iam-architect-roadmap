---
title: Labs on a Zero Budget
parent: 9. Practice
nav_order: 5
---

# Labs on a Zero Budget

## The constraint isn't money

Enterprise IAM products cost more than an individual can spend, and their trials are short and sales-gated. That stops nobody from learning identity, because **every concept in Stages 1–2 can be practised with free software on a laptop.**

{: .architect }
> **What you cannot learn at home is what a 40,000-identity estate feels like** — the data quality problems, the political negotiations, the connector that fails silently for three weeks, the certification campaign nobody completes. That knowledge comes only from work. So use labs for **mechanisms** (how does the flow actually go? what breaks when I change this?) and use your job — whatever it is — for **scale and organisational reality**. People who try to learn everything from labs end up excellent at protocols and unprepared for the role.

---

## The core lab

Everything below runs in Docker on a laptop.

### Keycloak — the single most valuable thing to run

The best learning platform for identity protocols that exists, because every setting is visible and changeable.

```bash
docker run -p 8080:8080 \
  -e KC_BOOTSTRAP_ADMIN_USERNAME=admin \
  -e KC_BOOTSTRAP_ADMIN_PASSWORD=admin \
  quay.io/keycloak/keycloak:latest start-dev
```

**Exercises, in order:**

1. Create a realm, users, groups, roles. Note what maps to which [concept](../05-platform-landscape/00-vendor-neutral-map.md).
2. Register an OIDC client. Complete an authorization code + PKCE flow **by hand**, using the browser and `curl`. Decode every token at every step.
3. Register a SAML client. Do SP-initiated SSO. Decode the `SAMLRequest` and `SAMLResponse`.
4. **Break things deliberately:** change the client secret; tamper with a token's payload; set the wrong `redirect_uri`; skip `state`. Observe every error message — recognising them later is worth hours.
5. Federate Keycloak to another Keycloak (identity brokering). You've now built the [hub-and-spoke pattern](../02-identity-fundamentals/11-federation-patterns.md).
6. Connect Keycloak to an LDAP directory for user federation.
7. Configure fine-grained authorisation services; compare with an external PDP.
8. Register a passkey (WebAuthn) and inspect the ceremony in dev tools.
9. Configure token lifetimes, then measure your actual revocation latency.
10. Enable and read the event log. What does a failed login look like? A token exchange?

### A directory

```bash
# OpenLDAP
docker run -p 389:389 -p 636:636 \
  -e LDAP_ORGANISATION="Example" -e LDAP_DOMAIN="example.com" \
  -e LDAP_ADMIN_PASSWORD=admin osixia/openldap:latest
```

Or **Samba AD DC** for something closer to Active Directory, which lets you practise Kerberos, group scopes and DNS SRV records.

**Exercises:** build an OU tree; write increasingly specific `ldapsearch` filters; add a schema extension; test what happens above the page-size limit; observe replication lag with two instances; write a filter using AD's `LDAP_MATCHING_RULE_IN_CHAIN` if using Samba.

### A certificate authority

```bash
# step-ca — much friendlier than raw openssl
docker run -p 9000:9000 smallstep/step-ca
```

**Exercises:** issue a server cert and a client cert; build a chain; break the chain and read the error; test what happens when you omit the intermediate (works in a browser, fails from `curl` — the [classic trap](../01-it-fundamentals/05-pki-and-tls.md)); set up mTLS between two services; let a certificate expire and observe the failure.

### An OPA / Cedar policy engine

```bash
docker run -p 8181:8181 openpolicyagent/opa run --server
```

**Exercises:** model the same three rules in Rego and in Cedar; write unit tests for a policy; measure decision latency; make a policy change that silently widens access, then write the test that would have caught it.

---

## Free tiers worth using

| Service | What you get | Use it for |
|:--|:--|:--|
| **Microsoft Entra ID** free tier / dev tenant | A real tenant | Conditional Access basics, app registrations, Graph, hybrid concepts |
| **Okta Developer** | A free developer org | Real SaaS IdP behaviour, SCIM provisioning to sample apps |
| **Auth0 free tier** | CIAM patterns | Actions, rules, social login, passwordless |
| **AWS / GCP / Azure free tiers** | Cloud IAM | Policies, roles, workload identity federation, OIDC from GitHub Actions |
| **GitHub Actions** | CI/CD identity | **Do the OIDC-to-cloud federation exercise — it's the highest-value single lab in this list** |
| **webauthn.io** | Passkey testing | The WebAuthn ceremony, without building anything |
| **jwt.io** | Token inspection | Decoding (never paste a production token) |
| **SAML tracer** (browser extension) | Federation debugging | Reading real flows |

---

## The lab exercises that teach the most

Ranked by learning per hour:

**1. Do an OAuth flow entirely by hand.** Browser for the redirect, `curl` for the token exchange. Read every parameter. This single exercise makes the whole protocol concrete in a way no diagram does.

**2. GitHub Actions → cloud via OIDC federation.** Removes a stored secret, teaches trust conditions, and is directly applicable at work. Then deliberately misconfigure the `sub` condition and understand why it's dangerous.

**3. Build a tiny SCIM server.** Even a stub that accepts create/update/delete teaches more about provisioning than reading the RFC.

**4. Break token validation.** Write a validator, then defeat it: `alg: none`, algorithm confusion, missing `aud`. You'll never write a naive validator again.

**5. Manual role mining.** Generate a spreadsheet of 50 users × 30 entitlements with realistic messiness. Try to derive roles. Feel the ambiguity — **this is why role projects fail**, and no amount of reading conveys it.

**6. Design a JML on paper for a fictional company** with employees, contractors and a subsidiary. Include every edge case. The single highest-value non-technical exercise in this repo.

**7. Set up federation between two Keycloak instances**, then break it: expire the certificate, skew the clock, change the entity ID. Every failure you cause is one you'll recognise in production.

---

## Learning from real estates without a lab

The most valuable practice is often free and already around you:

- **Read your own organisation's identity configuration**, if you have access. What Conditional Access policies exist? How many Global Admins? When do the certificates expire?
- **Read incident write-ups.** Public breach reports where identity was the vector. For each: which control would have stopped it, and would you have designed that control in?
- **Read the specs.** RFC 6749 (OAuth), RFC 7519 (JWT), the OIDC Core spec, RFC 7644 (SCIM). Dense, and there is no substitute for having read the actual text once.
- **Read vendor documentation for a product you don't use.** Practise the [translation](../05-platform-landscape/00-vendor-neutral-map.md).
- **Join IDPro** and read the Body of Knowledge.
- **Answer questions** in community forums. Explaining something to a stranger exposes exactly what you don't understand.

---

## A 30-day lab plan

| Days | Focus |
|:--|:--|
| 1–3 | Keycloak up; realm, users, groups, roles |
| 4–7 | OIDC by hand — every parameter, every token, deliberately broken |
| 8–10 | SAML by hand — decode both messages, break the signature |
| 11–13 | LDAP: tree, filters, paging limits, schema |
| 14–16 | PKI: your own CA, chains, mTLS, expiry failure |
| 17–19 | Keycloak-to-Keycloak brokering; then break it three ways |
| 20–22 | Cloud IAM: policies, roles, least privilege from usage data |
| 23–25 | **GitHub Actions → cloud OIDC federation** |
| 26–27 | Policy engine: same rules in Rego and Cedar, with tests |
| 28–30 | Passkey registration and authentication; read the assertion |

Thirty days of one to two hours produces genuine mechanical fluency — the kind that means you can debug someone else's federation problem, which is a reputation-making skill.

---

**Next:** [Stage 10 — Reference](../10-reference/) →

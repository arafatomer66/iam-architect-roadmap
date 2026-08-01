---
title: Protocol Cheat Sheets
parent: 10. Reference
nav_order: 2
---

# Protocol Cheat Sheets

Condensed for revision and for interviews. Full treatment in [Stage 2](../02-identity-fundamentals/).

---

## SAML 2.0

**Flow (SP-initiated):** app → 302 to IdP with `SAMLRequest` → authenticate → auto-POST `SAMLResponse` to ACS → validate → session.

**Key parameters:** `SAMLRequest`, `SAMLResponse`, `RelayState` (deep-link context), `SigAlg`, `Signature`.

**Bindings:** HTTP-Redirect (deflated+base64 in URL, size-limited), HTTP-POST (auto-submitting form — the usual response binding), Artifact (reference resolved over a back channel), SOAP.

**Assertion contains:** Issuer · Signature · Subject/NameID · SubjectConfirmationData (`Recipient`, `NotOnOrAfter`, `InResponseTo`) · Conditions (`NotBefore`/`NotOnOrAfter`, `AudienceRestriction`) · AuthnStatement (`AuthnContextClassRef`, `SessionIndex`) · AttributeStatement.

**SP must validate:**
1. Signature verifies against the **expected key from metadata** (never an embedded cert)
2. The signed element is the element consumed *(XSW defence)*
3. `Issuer` matches
4. `Audience` matches this SP
5. `Recipient` matches the ACS URL
6. `NotBefore` / `NotOnOrAfter` current
7. `InResponseTo` matches an outstanding request
8. Assertion ID not replayed
9. Algorithm acceptable (no SHA-1)
10. XML parser hardened (no DTD/XXE)

**NameID formats:** `persistent` (opaque, pairwise — usually right) · `emailAddress` (changes) · `transient` (per session) · `unspecified`.

**Decode a redirect-binding request:** URL-decode → base64-decode → raw inflate (`zlib` with `-15`).

---

## OAuth 2.x

**Roles:** Resource Owner · Client (confidential/public) · Authorization Server · Resource Server.

**Authorization code + PKCE:**
```
GET /authorize?response_type=code&client_id=…&redirect_uri=…
    &scope=…&state=…&code_challenge=…&code_challenge_method=S256
→ user authenticates
← 302 redirect_uri?code=…&state=…
POST /token   grant_type=authorization_code&code=…&code_verifier=…
              (+ client auth)
← access_token, refresh_token
```

**Other grants:** client credentials (M2M) · device authorization (input-constrained) · refresh token. **Removed in 2.1:** implicit, ROPC.

**Client authentication:** `client_secret_basic/post` (weak) · `private_key_jwt` (strong) · `tls_client_auth` (mTLS) · workload identity federation (no stored secret).

**Security parameters:** `state` (CSRF) · `nonce` (OIDC replay) · PKCE `code_verifier`/`code_challenge` (code interception).

**Key RFCs:** 6749 core · 6750 bearer · 7636 PKCE · 7662 introspection · 7009 revocation · 8693 token exchange · 8705 mTLS · 9068 JWT profile · 9126 PAR · 9396 RAR · 9449 DPoP.

---

## OpenID Connect

**On top of OAuth:** add `scope=openid` → get an **ID token**.

**Discovery:** `GET /.well-known/openid-configuration` → endpoints, `jwks_uri`, supported algorithms.

**ID token claims:** `iss` · `sub` (stable subject) · `aud` (your client ID) · `exp` · `iat` · `auth_time` · `nonce` · `acr` (assurance) · `amr` (methods used) · `azp`.

**RP must validate:** signature (key from `jwks_uri`) · `iss` exact · `aud` contains you · `exp`/`iat` · `nonce` matches · **algorithm pinned server-side** · `acr`/`auth_time` if requested.

**Key accounts on `iss` + `sub`. Never on email.**

**Request parameters worth knowing:** `prompt=login|consent|none` · `max_age` (forces re-auth) · `acr_values` (demand assurance) · `login_hint`.

**Client patterns:** server-side web → code+PKCE, confidential · SPA → code+PKCE, or **BFF** with an HttpOnly cookie · native → system browser, never a webview · CLI → device code · service → client credentials (no OIDC).

---

## JWT

```
base64url(header) . base64url(payload) . base64url(signature)
```

**Registered claims:** `iss` `sub` `aud` `exp` `nbf` `iat` `jti`.

**Validation:** pin `alg` · resolve keys only from a configured source (never `jku`/`x5u`/embedded) · verify signature · check `iss`, `aud`, `exp`/`nbf` · check scope · optionally introspect.

**Attacks:** `alg: none` · RS256→HS256 confusion (public key as HMAC secret) · `kid` injection · missing `aud` check · unvalidated decode.

**Algorithms:** `RS256` (interop default) · `PS256` (better padding) · `ES256` (compact, preferred new) · `EdDSA` · `HS256` (shared secret — never for federation).

**Remember: signed, not encrypted.** Anything in the payload is readable by the holder.

---

## SCIM 2.0

**Endpoints:** `/Users` `/Groups` `/Me` `/ServiceProviderConfig` `/Schemas` `/ResourceTypes` `/Bulk`

**Operations:** `GET` `POST` `PUT` (full replace) **`PATCH`** (partial — what you want) `DELETE`

**Key fields:** `externalId` (**your** key — always set it) · `id` (theirs) · `userName` · `active` (deactivation) · `meta.version` (ETag)

**Enterprise extension:** `employeeNumber` `department` `manager` `costCenter` `division`

**Test before committing:** PATCH support (attributes **and** group membership) · what DELETE vs `active:false` actually does, including licensing · pagination beyond one page · rate limits under bulk load · whether `externalId` is stored and filterable.

---

## Kerberos

**Flow:** AS-REQ/REP (get TGT, once per logon) → TGS-REQ/REP (get service ticket, once per service) → AP-REQ/REP (per connection).

**Objects:** TGT (encrypted with `krbtgt` key) · service ticket (encrypted with the service's key) · SPN · PAC (group SIDs) · authenticator (timestamp — hence clock dependency).

**Constraints:** clock skew ≤5 min · group changes need a new ticket · PAC size limits cap group counts.

**Attacks:** Kerberoasting (SPN ticket cracked offline) · AS-REP roasting (no pre-auth) · golden ticket (`krbtgt`) · silver ticket (service key, **no KDC logs**) · pass-the-ticket · delegation abuse.

**Mitigations:** gMSAs · AES only, no RC4 · eliminate unconstrained delegation · tiered admin · rotate `krbtgt` twice on compromise.

---

## LDAP

**Operations:** Bind · Unbind · Search · Compare · Add · Modify · Delete · ModifyDN · Abandon · Extended.

**Search scopes:** `base` · `one` · `sub`.

**Filters (prefix notation):**
```
(&(objectClass=user)(department=Finance)(mail=*))
(|(sn=Smith)(sn=Smyth))
(!(userAccountControl:1.2.840.113556.1.4.803:=2))    # not disabled (AD bitwise AND)
(memberOf:1.2.840.113556.1.4.1941:=CN=Grp,…)          # nested membership (AD in-chain)
```

**Escape in filters:** `*`→`\2a` `(`→`\28` `)`→`\29` `\`→`\5c` NUL→`\00`

**Controls to know:** paged results (RFC 2696 — **AD default page size 1000**) · DirSync (AD incremental) · content sync (RFC 4533) · password policy.

**Ports:** 389 LDAP · 636 LDAPS · 3268/3269 Global Catalog.

**Remember: DN is a location, not an identifier.** Correlate on `entryUUID` / `objectGUID`.

---

## FIDO2 / WebAuthn

**Registration:** RP sends challenge + `rp.id` + `authenticatorSelection` → `navigator.credentials.create()` → authenticator generates a key pair bound to the RP ID → RP stores credential ID + public key.

**Authentication:** RP sends challenge + `allowCredentials` → `navigator.credentials.get()` → authenticator signs (challenge ‖ **origin** ‖ flags ‖ counter) → RP verifies.

**Why it's phishing-resistant:** the authenticator has no key for the wrong origin, and the browser supplies the origin. The user cannot override it.

**Decisions:** RP ID (highest domain you'll keep — **one-way door**) · `userVerification: required` for two-factor in one gesture · resident/discoverable credentials for usernameless · attestation (`none` for consumer, `direct`/`enterprise` to restrict models) · **always ≥2 credentials per user**.

---

## Quick command reference

```bash
# Decode a JWT payload (inspection only — never a trust decision)
echo "$JWT" | cut -d. -f2 | tr '_-' '/+' | base64 -d 2>/dev/null | jq .

# Inspect a certificate
openssl x509 -in cert.pem -noout -text
openssl s_client -connect host:443 -servername host </dev/null | openssl x509 -noout -dates -subject -ext subjectAltName

# Verify a chain
openssl verify -CAfile chain.pem cert.pem

# LDAP search
ldapsearch -H ldaps://dc:636 -D "cn=svc,dc=x,dc=y" -W -b "dc=x,dc=y" "(sAMAccountName=jsmith)" memberOf

# OIDC discovery
curl -s https://idp.example.com/.well-known/openid-configuration | jq .

# Decode a SAML redirect request
python3 -c "import base64,zlib,sys,urllib.parse; print(zlib.decompress(base64.b64decode(urllib.parse.unquote(sys.argv[1])),-15).decode())" "$REQ"
```

---

**Next:** [Standards & RFC Index](03-standards.md) →

---
title: Reading & Following
parent: 10. Reference
nav_order: 6
---

# Reading & Following

## The primary sources

Read these once properly; they outrank every summary:

- **IDPro Body of Knowledge** — the profession's own reference, vendor-neutral, peer-reviewed, and closest in spirit to this repo. Start here.
- **NIST SP 800-63** (Digital Identity Guidelines) — the IAL/AAL/FAL vocabulary. The most useful single document in identity, whether or not NIST applies to you.
- **NIST SP 800-207** (Zero Trust Architecture) — what Zero Trust actually means, without the marketing.
- **RFC 9700** (OAuth Security Best Current Practice) — consolidated current guidance; supersedes a lot of older advice.
- **RFC 8725** (JWT Best Current Practices) — the attack list and mitigations.
- **The OpenID Connect Core specification** — denser than the summaries, and worth one careful pass.
- **The Zanzibar paper** (Google) — even if you never build ReBAC, it will change how you think about authorisation data.

---

## Books

Identity has few genuinely good books, and several that are product manuals in disguise. Worth your time:

- **Solving Identity Management in Modern Applications** (Wilson & Hingnikar) — the best practical treatment of OAuth/OIDC in real applications.
- **Identity Management: A Business Perspective** (Williamson) — the business framing that engineers usually lack.
- **Zero Trust Networks** (Gilman & Barth) — the architectural reasoning, not the vendor version.
- **Practical Cryptography for Developers / Serious Cryptography** (Aumasson) — enough crypto to make good decisions without becoming a cryptographer.
- **Building Secure and Reliable Systems** (Google SRE series) — free online; the chapters on access control and least privilege are excellent.
- **The Phoenix Project / The DevOps Handbook** — not identity, but the best available explanation of why "make the right path the fast path" beats gatekeeping.

Adjacent but formative: **Thinking in Systems** (Meadows) for how to reason about feedback loops, which is most of what an identity estate is.

---

## Following the field

**Standards and community:**
- IETF **OAuth working group** mailing list — where the next thing is argued about years before it's a product.
- **OpenID Foundation** working groups — especially Shared Signals, FAPI and OID4VC.
- **W3C WebAuthn** and **Verifiable Credentials** working groups.
- **IDPro** — the professional body: Slack community, BoK, conference.

**Conferences worth the time:**
- **Identiverse** — the largest identity-specific conference.
- **European Identity & Cloud Conference (EIC)** — the European counterpart, strong on governance and regulation.
- **Gartner IAM Summit** — analyst-led, useful for market shape and for executive framing.
- **Internet Identity Workshop (IIW)** — unconference format, where decentralised identity and new ideas get argued out. Unusually high signal.
- Vendor conferences (SailPoint Navigate, Ping YOUniverse, Okta Oktane) — product roadmaps and, more usefully, other customers.

**Reading regularly:**
- Vendor engineering blogs — Okta's developer blog, Ping's technical content, Microsoft's Entra blog, Auth0's blog. Marketing-adjacent but often technically solid.
- **Public incident write-ups.** The single best source of learning in this field. When an identity-related breach is disclosed, read the technical detail and ask: *which control would have stopped this, and would I have designed it in?*
- **Vendor security advisories and release notes** for products you run. A scheduled task, not an ad-hoc one.
- **MITRE ATT&CK** updates for credential access, persistence and privilege escalation techniques.

---

## A sustainable habit

The field moves fast enough that "keeping up" is a real time cost. What works:

| Cadence | Activity |
|:--|:--|
| **Weekly** (30 min) | Skim one vendor blog and any advisories for products you run |
| **Monthly** (2 hrs) | Read one specification, standard or long-form piece properly. Write six sentences on it |
| **Quarterly** (half a day) | Review your own estate against something you learned. Update one design or standard |
| **Annually** | One conference or one certification. Re-take the [self-assessment](../00-start-here/04-self-assessment.md) |

**Write as you learn.** A short internal post or a public explainer per month does three things: it exposes shallow understanding immediately, it makes the knowledge durable, and it builds the reputation that brings opportunities. Most well-known identity practitioners became well known by explaining things clearly, not by knowing more than everyone else.

---

## A caution about sources

{: .warning }
> **A large share of identity content is vendor marketing wearing a technical costume** — especially on the [frontier topics](../08-frontier/). Anything claiming a category is "dead", anything with a maturity model whose top tier requires their product, anything defining Zero Trust as a thing you can buy. **Test every claim against the question: what assumption does this address, and what breaks if I ignore it?** If the answer isn't clear in plain language, you're reading positioning, not analysis.

The corollary: prefer **specifications, incident reports and practitioner accounts** over analyst summaries and vendor whitepapers. Primary sources are slower to read and worth several times as much.

---

## If you only do three things

1. **Read the IDPro Body of Knowledge and NIST 800-63.**
2. **Read every public identity breach write-up**, and ask what you'd have designed differently.
3. **Write one explanation a month** of something you just learned.

That's perhaps three hours a month, and it will keep you ahead of most of the field — because most of the field does none of it.

---

## Where to go from here

You've reached the end of the repo. If you started at [Stage 1](../01-it-fundamentals/), you now have the material. What turns it into competence:

- Do the [design exercises](../09-practice/02-design-exercises.md) — properly, in writing.
- Re-take the [self-assessment](../00-start-here/04-self-assessment.md) and compare with your first attempt.
- Pick the two lowest competencies and go back to those stages.
- Find something real to own, and live with your decisions long enough to learn from them.

> **The tools will change. The principles won't.**

---

*Corrections and additions welcome — see [CONTRIBUTING](../CONTRIBUTING.md).*

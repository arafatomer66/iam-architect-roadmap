---
title: Interview Prep
parent: 9. Practice
nav_order: 4
---

# Interview Prep

## What they're actually assessing

For an architect role, interviewers are testing four things, roughly in this order of weight:

1. **Do you reason, or do you recall?** Given an unfamiliar problem, can you decompose it? Recall is table stakes; reasoning is the role.
2. **Do you know the failure modes?** Anyone can describe a happy path. Knowing what breaks — and why — is what experience sounds like.
3. **Can you communicate at different altitudes?** The same design, explained to an engineer and to a CFO.
4. **Have you lived with your own decisions?** Have you been wrong, noticed, and changed course?

{: .architect }
> **The single strongest signal you can give is to volunteer a trade-off before you're asked.** "I'd use JWTs here for the latency, and I'll be explicit that this means I can't revoke inside the token lifetime — so if the revocation SLA is under five minutes, I'd introspect on the sensitive endpoints instead." That sentence demonstrates mechanism knowledge, awareness of consequences, and a decision framework, all at once. Candidates who present designs without downsides read as either inexperienced or as selling.

---

## Technical questions, and what a strong answer contains

**"Walk me through what happens when a user clicks 'Log in with SSO'."**
Draw the redirect chain. Name where the assertion is signed and what the SP validates. Mention `RelayState` and `InResponseTo`. Distinguish IdP session from application session. A strong answer notes that the IdP and SP may have no direct network path, because the browser carries the assertion.

**"What's the difference between OAuth and OIDC?"**
Not "OIDC is OAuth for authentication" and stop. Explain the *problem*: OAuth delegates authorisation and says nothing about who the user is; using an access token as proof of login is the confused-deputy/token-substitution flaw; OIDC's ID token has an `aud` claim naming the client, which is what fixes it.

**"How would you validate a JWT?"**
The full list, and — more importantly — "**the token must never determine how it's validated**." Name `alg: none`, algorithm confusion, and unvalidated `jku`/`kid`.

**"Why is single logout hard?"**
Three session layers. Front-channel breaks on third-party cookie restrictions; back-channel needs connectivity and an implemented endpoint on every RP. The honest conclusion: treat it as best-effort and rely on short sessions plus revocation.

**"RBAC or ABAC?"**
Both, layered, with the reason: RBAC is certifiable (you can list who has it), ABAC handles context but can't easily answer "who has access?", which auditors require. A strong answer names role explosion and ABAC's audit problem without prompting.

**"How do you handle a leaver?"**
Disable authentication *first*. Then sessions and tokens. Then entitlements. Then shared secrets they knew. Then owned objects. Then evidence. Mention that the SLA must be measured from the real-world event, not from when your system got the message.

**"What's your approach to service accounts?"**
Owner, purpose, expiry, rotation — and the destination is no stored credential at all via workload identity federation. Mention that discovery is continuous, not a project.

**"How would you secure the identity provider itself?"**
Tier 0. Separate admin identities, phishing-resistant MFA, JIT elevation, non-exportable keys, immutable log export, configuration baseline, and detections for new signing keys and new federation trusts.

---

## Architecture and judgement questions

**"Tell me about a design decision you got wrong."**
The question that most reliably separates architects from senior engineers. A strong answer: a real decision, the reasoning that made it defensible *at the time*, what you learned, what you changed, and how it altered how you decide now. A weak answer is a disguised strength or blames someone else. **Prepare one properly** — this question is nearly universal.

**"How do you decide between building and buying?"**
Should include: total cost including the run team, whether the capability is differentiating for your business, the exit cost, and the team's capacity to own it. A strong answer names a case where they chose each way.

**"How do you get a design implemented when you have no authority?"**
Influence, not gatekeeping. Give the reasoning, let people shape it, make the right path the fast path, pick your battles, be consistent and write things down.

**"What would you do in your first 90 days?"**
Listen, measure, find the baseline numbers, identify the quick win, meet HR and the service desk, read the audit findings. **Do not propose an architecture before you understand the estate** — proposing one in week one is a red flag to good interviewers.

**"How do you keep current?"**
Specifics: standards bodies, IDPro, incident write-ups, vendor release notes, hands-on labs. Vague answers ("I read a lot") suggest you don't.

---

## Questions about scale and pressure

**"What's the largest environment you've worked in?"** Be exact and honest. Overstating is easily caught by follow-ups about what breaks at that scale.

**"Tell me about an outage you were involved in."** What happened, what you did, what you changed afterwards. Owning a mistake is a positive signal; deflecting is not.

**"How do you handle disagreement with a senior stakeholder?"** State the risk factually, offer alternatives, document the acceptance, escalate rarely. Show you can lose gracefully — architects who treat every disagreement as a battle don't last.

---

## Questions to ask them

These signal seniority, and they get you information you need:

- **"Who owns the decision about who gets access to an application here?"** — reveals whether ownership exists.
- **"How long does leaver revocation take today, end to end?"** — reveals whether they measure anything.
- **"What's the exception queue like?"** — reveals operational maturity instantly.
- **"Is the run team funded separately from the project?"** — reveals whether the programme has a future.
- **"What happened in your last identity-related incident?"** — reveals culture and honesty.
- **"What would you want me to have changed in twelve months?"** — reveals whether they have a coherent expectation of the role.
- **"Who would I disagree with most, and how does that get resolved?"** — an unusually revealing question that people answer candidly.

---

## Preparation checklist

**Two weeks out:**
- [ ] Draw SAML, OIDC and Kerberos flows from memory until it's automatic.
- [ ] Prepare your "decision I got wrong" story properly.
- [ ] Prepare three war stories: an incident, a migration, a stakeholder conflict.
- [ ] Know your numbers — sizes, populations, timescales of what you've worked on.
- [ ] Practise [whiteboard scenarios](03-whiteboard-scenarios.md) out loud, timed.

**One week out:**
- [ ] Research the company: sector, regulation, likely estate, any public incidents.
- [ ] Look up their probable stack (job ad, LinkedIn, engineering blog) and read the [vendor page](../05-platform-landscape/) for it.
- [ ] Prepare your questions.
- [ ] Practise explaining a design with no protocol names.

**The day:**
- [ ] Clarify before designing. Always.
- [ ] Think out loud.
- [ ] Volunteer trade-offs.
- [ ] Say "I don't know" when you don't — then say how you'd find out. **This is a strong answer, not a weak one.** Confidently wrong is disqualifying; honestly uncertain with a method is not.

---

## The thing that most often costs people the offer

Answering the question they *expected* rather than the one asked. An interviewer asking "how would you approach IGA here?" is not asking for a description of IGA. They're asking what you would do *in their situation* — which means asking about their situation first.

**Clarify. Then answer. Then say what you'd want to find out.**

---

**Next:** [Labs on a Zero Budget](05-labs-on-a-budget.md) →

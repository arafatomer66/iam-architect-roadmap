---
title: Whiteboard Scenarios
parent: 9. Practice
nav_order: 3
---

# Whiteboard Scenarios

Timed, verbal, interview-shaped. **Say your answer out loud**, with a marker or a blank page, in the time given. The constraint is the point.

---

## How to run a whiteboard well

The structure that works, in roughly this proportion of the time:

1. **Clarify (10%).** Ask three or four questions before drawing anything. *Who are the users? What's the scale? What's driving this — audit, breach, cost, a new product? What exists today? Any hard constraints?* **Interviewers score this heavily** — an architect who starts drawing immediately is telling you they don't gather requirements.
2. **Frame (10%).** State the problem back in one or two sentences, and name the two or three things that will drive the design. This shows you've understood rather than pattern-matched.
3. **Draw (50%).** Start with boxes and flows, not products. Name the authoritative sources first, then the runtime path, then governance.
4. **Deepen (20%).** Pick the one or two places with real difficulty and go deeper. Volunteer the trade-off you're making.
5. **Close (10%).** State what you'd do first, what you deliberately left out, and what you'd need to find out.

**Say your reasoning out loud continuously.** The interviewer is assessing how you think, and silence gives them nothing to assess. "I'm putting the authoritative source here because..." is the entire skill on display.

---

## Scenario 1 — Design workforce IAM for a 20,000-person retailer (30 min)

**Expect follow-ups on:** seasonal workers (a large, high-churn population with a hard end date — do you handle them differently?), store-floor shared devices, franchise staff who aren't your employees, and the fact that head office and store populations have completely different access patterns.

**Strong answers** distinguish the populations early, handle seasonal staff with automatic expiry rather than a leaver process, address shared devices realistically (badge tap, fast switching) rather than banning them, and treat franchise staff as [B2B](../03-identity-domains/03-b2b-identity.md) rather than as employees.

---

## Scenario 2 — Our IdP is down. Walk me through what happens (20 min)

**What's being tested:** whether you've thought about failure at all.

**Cover:** who can still work (existing sessions? cached credentials? which applications hold their own session?), what's completely blocked, break-glass, how you'd know, the communication path if chat and email authenticate through the same IdP, and the recovery sequence.

**Strong answers** distinguish between *existing sessions continuing* and *new authentications failing*, mention that the incident communication channel must not depend on the failed system, and note that the post-recovery thundering herd can take the service down again.

---

## Scenario 3 — Design CIAM for a bank's new mobile product (40 min)

**Expect follow-ups on:** KYC and regulatory proofing, PSD2 strong customer authentication and dynamic linking, how you'd do passkeys with a fallback that isn't the weak link, account recovery without a branch visit, and fraud.

**Strong answers** treat proofing as pluggable, discuss the conversion/security trade-off with numbers rather than assertion, notice that recovery becomes the attack path once authentication is strong, and separate the CIAM estate from workforce identity.

---

## Scenario 4 — How would you know if your IdP admin account was compromised? (20 min)

**What's being tested:** detection thinking, and whether you know identity persistence mechanisms.

**Cover:** the detections (new signing key, new federation trust, consent grants, CA exclusions, role assignments, MFA registrations), where logs go and whether an admin could alter them, the configuration baseline, and the honest answer about what you *couldn't* detect.

**Strong answers** volunteer that disabling the account isn't enough, and that the hunt is for what was *created*.

---

## Scenario 5 — Explain OAuth to a project manager (10 min)

**What's being tested:** communication, not knowledge.

**Strong answers** use an analogy (a hotel key card that opens some doors, for a limited time, and can be cancelled without changing the locks), avoid the words "grant type" and "bearer token", and check understanding partway through. **Weak answers** enumerate the four roles and the flows.

Variant: *explain the difference between authentication and authorisation to a board member.* Ten seconds, no jargon.

---

## Scenario 6 — We have 300 applications and no SSO. Where do you start? (25 min)

**Expect follow-ups on:** how you'd prioritise, what you'd do about applications that can't federate, how long it would take, and how you'd resource it.

**Strong answers** risk-rank rather than starting alphabetically or with the easiest, propose a tiered onboarding standard so the fast path is genuinely fast, name the proxy pattern for legacy, and give a range with the reasoning rather than a confident single number.

---

## Scenario 7 — Design privileged access for a cloud-native company (30 min)

**Expect follow-ups on:** whether you'd buy a vault at all, how CI/CD fits, what "privileged" means when infrastructure is code, and how you'd govern the repository.

**Strong answers** notice that the IaC repository is now an identity control plane, propose native JIT for cloud roles rather than reflexively buying a vault, and address the developer-culture constraint directly.

---

## Scenario 8 — Your provisioning connector has been silently failing for three weeks (20 min)

**What's being tested:** operational thinking and honesty.

**Cover:** how you'd establish the blast radius (who *should* have been provisioned or deprovisioned in that window), how you'd remediate, why it was silent, and what you'd change so it can't be silent again.

**Strong answers** immediately ask whether the failures were *grants* or *revocations* — because three weeks of failed revocations is a security incident with a notification question, while three weeks of failed grants is an annoyance. Recognising that distinction quickly is a strong signal.

---

## Scenario 9 — Sell an IAM programme to a sceptical CFO (15 min)

**What's being tested:** business translation. Expect them to play the sceptic properly.

**Strong answers** lead with money and risk, use real numbers with the method shown, name what happens if nothing is done, and make a specific ask. **Weak answers** mention a protocol, quote a vendor, or say "best practice".

---

## Scenario 10 — Design identity for AI agents acting on employees' behalf (30 min)

**Increasingly common, and it separates candidates sharply.**

**Cover:** the agent's own identity, delegation via token exchange preserving both subject and actor, authority as an intersection, short scoped tokens, the human-approval threshold, attribution in logs, the kill switch, and what happens when the delegating user leaves.

**Strong answers** raise the confused-deputy/prompt-injection risk unprompted and address it architecturally — by constraining authority rather than by trusting the prompt layer.

---

## Practising alone

- **Record yourself.** Uncomfortable, and by far the fastest way to find your filler words, your rambling, and the point at which you stopped explaining.
- **Time strictly.** Running out of time before reaching governance is a common and fatal pattern.
- **Draw on paper**, not in a diagramming tool. Whiteboards are messy; practise messy.
- **Have a default structure** you can fall back on when a question surprises you: *sources → identity store → runtime → governance → privileged → what breaks.* It works for almost any workforce question and it stops you freezing.
- **Practise the close.** "What I'd do first, what I left out, what I'd need to find out" is a strong ending and almost nobody does it.

---

**Next:** [Interview Prep](04-interview-prep.md) →

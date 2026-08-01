---
title: Case Studies
parent: 9. Practice
nav_order: 1
---

# Case Studies

Five worked situations. **Spend fifteen minutes on your own answer before reading the discussion.** All are composites; any resemblance to a specific organisation is coincidental.

---

## Case 1 — The audit finding

> **Situation.** A 14,000-person manufacturer, listed in the US, failed a SOX audit. The finding: *"Management cannot demonstrate that access to financial systems is reviewed periodically, or that terminated employees' access is removed promptly."* Sampling found 6 of 25 leavers still had active accounts 30+ days later. There is no IGA platform. HR is SAP SuccessFactors, the directory is on-prem AD with Entra ID sync, and there are roughly 180 applications, of which 40 are finance-relevant. The CFO has asked for a plan in three weeks. Remediation is due in nine months.

### Discussion

**Understand the actual problem.** The finding is about *evidence*, not only about access. Even if leaver processing improved tomorrow, they'd fail again next year without a way to demonstrate it. Two deliverables, then: fix the control, and make it evidential.

**The nine-month constraint drives the sequence.** A full IGA programme won't be delivered in nine months. Something demonstrable must be, so scope hard.

**Proposed sequence:**

1. **Weeks 1–4 — measure and stop the bleeding.** Establish the real leaver revocation number (it will be worse than 30 days at p95). Build a weekly reconciliation of AD accounts against HR-active identities, and hand the delta to a named owner. **This alone is a detective control you can show an auditor**, and it can be running in a fortnight with a script.
2. **Months 1–4 — automate the leaver for the 40 in-scope applications.** Not all 180. HR event → disable AD → revoke the finance applications. Evidence captured per user.
3. **Months 3–7 — certification for the 40.** Application owners identified (this is the political work; start it in week two), entitlements described in business language, first campaign run.
4. **Months 6–9 — evidence package and audit rehearsal.** Run the auditor's own test on yourself before they do.
5. **Post-remediation** — extend to the remaining 140 applications, then access request, then roles.

**What an architect adds beyond the plan:**

- Points out that **scope is the lever**: 40 applications is defensible to the auditor if the risk-based rationale is documented; 180 is undeliverable and the attempt would fail both.
- Insists on **measuring end to end from the HR event**, because the delay is probably upstream in HR data entry, and only that measurement makes it visible.
- Names **application owner identification** as the critical path — it is, and it's usually discovered too late.
- Warns that the tooling decision should not precede the process definition, while acknowledging the CFO wants to see a purchase. Suggests the reconciliation control (which needs no product) as the visible early action.

**The trap:** buying a platform in month one to demonstrate action, then discovering in month five that nobody owns the entitlements and no data supports certification.

---

## Case 2 — The acquisition

> **Situation.** A 9,000-person financial services firm is acquiring a 3,500-person competitor, completing in 90 days. The acquirer runs Entra ID with on-prem AD and PingFederate for partner federation. The target runs its own AD forest, Okta, and a Google Workspace tenant. Both are regulated. Day one requires email, chat and file sharing between the organisations. Full integration is expected within two years.

### Discussion

**Three horizons, three different right answers** ([migration](../04-architecture-practice/04-migration-and-coexistence.md)).

**Day one** — do *not* attempt consolidation. Cross-tenant collaboration: Entra B2B guest access for target staff into acquirer collaboration tools and vice versa, or federated chat and calendar sharing. Two IdPs, both live, no directory merge. This is achievable in 90 days; a directory merge is not.

**Days 30–120** — the things that create risk if deferred:
- **A single authoritative identifier** across both populations, even though nothing else merges. Without it you cannot later reconcile, and you will not know who exists twice.
- **A combined leaver process with an explicit owner.** During transitions, leavers routinely stay live in the other estate because nobody owned it. This is the single highest risk in the interim period and it is an organisational fix, not a technical one.
- **Identify people who exist in both organisations** — it's a regulated sector with a competitor, so there will be some, often senior.
- **Privileged access review on both sides.** M&A is when administrator populations are least controlled.

**Year 1–2** — target state. Likely: acquirer's Entra as the surviving IdP, PingFederate retained for partner federation (a genuine capability the target's Okta doesn't replace), target's AD consolidated by cohort, Google Workspace either retained as a business decision or migrated. Broker in front of both IdPs during transition, **with an explicit decommission date**.

**What an architect adds:**

- Refuses the natural instinct to design only the end state, and makes the interim explicit.
- Flags the **regulatory dimension**: both entities are regulated, so access evidence must be maintained *throughout* the transition, including for the target's systems the acquirer doesn't yet understand.
- Names the broker as **temporary infrastructure with a date**, because that's the artefact most likely to become permanent.
- Raises **data residency and lawful basis** if the entities are in different jurisdictions.

---

## Case 3 — The CIAM rebuild

> **Situation.** A retailer with 12 million registered customers runs a homegrown authentication system built in 2013: passwords with bcrypt, a MySQL user table, no MFA, session cookies with a 30-day lifetime, and account recovery by emailed reset link. Credential stuffing is costing an estimated €2m/year in fraud and support. Marketing wants social login and a single view of the customer across web and app. Legal has flagged that consent is stored as three boolean columns with no history. Registration completion is 61%.

### Discussion

**Note that four different stakeholders want four different things** — security wants stuffing stopped, marketing wants conversion and unified profiles, legal wants defensible consent, engineering wants to stop maintaining 2013 code. A good architecture serves all four, and framing it that way is how it gets funded.

**Priority order, by value and risk:**

1. **Breached-credential screening and bot management** — deployable in weeks, directly attacks the €2m. Do this before any re-platform; it's the highest ROI action available and it buys credibility.
2. **Passkeys** — attacks stuffing structurally, *improves* conversion (faster than passwords), and reduces reset volume. Offer alongside passwords; promote at login.
3. **Consent model** — versioned, per-purpose, with timestamps and channel. **This cannot be retrofitted historically**, so the sooner it starts the smaller the unreconstructable gap. Legal's concern is well-founded.
4. **Session redesign** — 30 days is too long; step-up for payment and profile changes; notify the *old* contact details on changes.
5. **Platform decision** — build vs buy. With 12 million users and a small team, buy.
6. **Account linking** — for social login, linking only on *verified* email.

**What an architect adds:**

- Sequences the **cheap high-value controls before the re-platform**, rather than making everything wait on a procurement.
- Insists security changes be justified with **conversion data**, not assertion — this is CIAM, where the business will veto anything that costs revenue unless shown the fraud trade ([CIAM](../03-identity-domains/02-ciam.md)).
- Flags **migration of 12 million password hashes** as a specific design problem: bcrypt hashes can often be migrated into a new platform, but verify it, and treat it as an opportunity to enrol passkeys on next login.
- Notes that **recovery via emailed link** is now the weakest path and will become the account-takeover route once passwords are strengthened.

---

## Case 4 — The cloud sprawl

> **Situation.** A 2,000-person software company. Engineering has 340 AWS accounts, three GCP projects and an Azure subscription. Access is granted through Terraform in each team's repository. There are 1,100 IAM users with long-lived access keys, some in code. Nobody can answer "who can access production customer data?" A SOC 2 audit is in five months. Engineering culture is strongly anti-process.

### Discussion

**The cultural constraint is the design constraint.** Any solution requiring engineers to raise tickets will be routed around, and the architect who proposes it loses credibility permanently.

**The approach that works here:**

1. **Federate humans, eliminate IAM users.** SSO into IAM Identity Center with permission sets mapped to IdP groups. Removes the 1,100 static credentials and gives a single answer to "who can access what".
2. **CI/CD via OIDC federation** — no stored keys. Pin trust conditions to specific repositories and refs ([workload identity](../03-identity-domains/05-workload-identity.md)).
3. **Secret scanning in the pipeline**, blocking on commit, plus history and image scanning. Rotate everything already exposed — assume compromise.
4. **Guardrails, not gates.** SCPs at the organisation level, policy-as-code checks on Terraform PRs. Engineers keep their workflow; the controls are in the pipeline.
5. **JIT for production access**, with an approval that takes seconds, not a ticket queue. If elevation is fast, people use it.
6. **CIEM** for the "who can access production data?" question, since cloud permissions are computed and can't be listed.
7. **Governance rides on the IdP groups**, which are certifiable, rather than attempting to certify IAM policy JSON.

**What an architect adds:**

- Recognises that **the repository is now the access control plane**, so repository permissions and branch protection are identity controls and must be governed as such.
- Frames the whole thing as **removing toil** — no more key rotation, no more "who has the credentials" — rather than as adding process. Same controls, different framing, entirely different reception.
- Points out that SOC 2 will ask for access reviews, and IdP group membership is reviewable in a way that IAM policies are not — solving the audit requirement and the architecture requirement together.

---

## Case 5 — The failing programme

> **Situation.** You join an organisation 18 months into a €4m IGA implementation. Twelve of 120 planned applications are connected. The role model has 2,300 roles for 8,000 employees, most with one member. Certification campaigns complete at 98% with a 0.3% revocation rate. The system integrator's contract ends in three months. The sponsor is asking whether to cancel.

### Discussion

**Diagnose before prescribing.** Each symptom points somewhere specific:

- **12 of 120 applications** → integration was underestimated, or the hard ones were left to last, or connector work is blocked on missing test environments. Find out which.
- **2,300 roles, mostly single-member** → the role model was built top-down without data and is modelling individuals ([role modelling](../02-identity-fundamentals/16-role-modelling.md)). It is not salvageable as-is.
- **98% completion, 0.3% revocations** → rubber-stamping. The campaigns are producing evidence of a control that isn't operating, which is *worse than not running them* from an audit perspective.
- **Sequence** → they almost certainly started with roles instead of leaver automation, which is the classic failure.

**What you'd recommend:**

1. **Do not cancel.** €4m of platform and connector work exists. Re-scope instead.
2. **Freeze the role model.** Stop adding to it. Keep roles that have >5 members and a real owner; convert the rest to direct assignments with expiry. This is unpopular and correct.
3. **Re-sequence to value:** leaver automation across the 12 connected applications *now*, plus the reconciliation control. Something demonstrable within 60 days.
4. **Fix certification**: risk-scope it to the top 20 entitlements, add business descriptions and last-used data, and report **revocation execution** alongside completion. A campaign with 12% revocations on 200 items is worth more than 98% completion on 40,000.
5. **Knowledge transfer from the SI as a contractual deliverable** in the remaining three months — this is urgent and the window is closing.
6. **Hire the run team now.** If there's no internal capability when the SI leaves, the programme dies regardless of scope.
7. **Reset expectations with the sponsor honestly**: here's what €4m bought, here's what the next 12 months delivers, here's what we're abandoning and why.

**What an architect adds:** the willingness to say the role model was a mistake, and to propose abandoning sunk work. Everyone else in the room is invested in it. **That's the job** — and doing it with evidence rather than blame is what makes it land.

---

**Next:** [Design Exercises](02-design-exercises.md) →

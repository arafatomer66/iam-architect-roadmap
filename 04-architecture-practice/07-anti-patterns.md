---
title: Anti-Patterns
parent: 4. Architecture Practice
nav_order: 7
---

# IAM Anti-Patterns

Every one of these is **tempting for a good reason**. That's what makes them anti-patterns rather than obvious mistakes — each solves a real, immediate problem while creating a larger one later. Recognising the temptation is how you counter it, so each entry names it.

---

## Data and model

### 1. The DN (or email) as a foreign key

**Tempting because:** it's right there, it's human-readable, and it works in testing.
**Breaks when:** someone marries, moves department, or the company rebrands its domain. Every downstream reference is now wrong, and the failure is silent.
**Instead:** correlate on an immutable identifier (`objectGUID`, `entryUUID`, employee ID, `iss`+`sub`). Carry email as an attribute.

### 2. Cleansing data inside the IAM platform

**Tempting because:** the source owner won't fix it and you need to ship.
**Breaks when:** the next sync overwrites your fix, or doesn't — and now two systems disagree permanently, with no one able to say which is right.
**Instead:** quarantine and report to the source owner, with the impact attributed. See [data quality](../02-identity-fundamentals/19-identity-data-quality.md).

### 3. One identity per account

**Tempting because:** the initial data has one row per system user, and modelling personas looks like over-engineering.
**Breaks when:** someone is an employee *and* a contractor for a subsidiary *and* an administrator with a separate privileged account. You now have three unlinked identities for one human, and your leaver process catches one of them.
**Instead:** model identity, account and persona as distinct concepts from day one.

---

## Authentication and access

### 4. MFA at the perimeter only

**Tempting because:** one integration point (the VPN) covers everyone at once.
**Breaks when:** anything is reachable without the VPN — which is now everything — or when an attacker gets inside once.
**Instead:** MFA at the application/identity layer, everywhere. See [Zero Trust](../08-frontier/01-zero-trust.md).

### 5. Permanent MFA exemptions

**Tempting because:** an executive, a legacy app or a critical service genuinely can't do it *today*.
**Breaks when:** the exception list only grows, and it precisely enumerates your soft targets for anyone who obtains it.
**Instead:** every exemption gets an owner, a compensating control and an **expiry date**. Report the count as a KPI.

### 6. Header-based SSO without network isolation

**Tempting because:** it makes an unmodifiable legacy app work in an afternoon.
**Breaks when:** anything can reach the application directly and simply *sends the header* — instant impersonation of any user.
**Instead:** enforce that the application is only reachable through the proxy, and verify it. If you can't, treat the app as an accepted risk on the register.

### 7. Long-lived tokens because refresh is inconvenient

**Tempting because:** it removes a class of user-visible errors and support tickets.
**Breaks when:** revocation is required and the token has eleven hours left. Your revocation SLA is now fiction.
**Instead:** short access tokens plus refresh rotation, and know your actual revocation latency ([tokens](../02-identity-fundamentals/06-tokens-and-jwt.md)).

---

## Authorisation and roles

### 8. Role per person

**Tempting because:** every request for an exception is easiest to satisfy with a new role.
**Breaks when:** you have more roles than users, nobody can explain the model, and the role layer adds overhead without adding comprehension.
**Instead:** composition (several roles per person), attributes for dimensional data, and a hard rule that every role needs an owner and a business description. See [role modelling](../02-identity-fundamentals/16-role-modelling.md).

### 9. Deep group nesting

**Tempting because:** it feels like elegant reuse, and each individual nesting is sensible.
**Breaks when:** four levels down, nobody can answer "why does this person have access?", different applications resolve nesting differently (some not at all), and Kerberos tickets exceed size limits.
**Instead:** maximum two levels, every group owned and purposeful.

### 10. Authorisation only in the UI

**Tempting because:** hiding the button demonstrably stops users doing the thing.
**Breaks when:** anyone calls the API directly — which is the actual interface.
**Instead:** enforce in the service, at object level. The UI is a usability affordance, never a control.

### 11. The convenient super-role

**Tempting because:** support genuinely needs broad access to help customers, and scoping it is real work.
**Breaks when:** it leaks, is phished, or is misused — and it has access to everything, for everyone.
**Instead:** scoped, time-bound, justified access with recording. "Read this customer's data for this ticket, for two hours."

---

## Process and governance

### 12. Provisioning without reconciliation

**Tempting because:** provisioning delivers visible value and reconciliation delivers uncomfortable findings.
**Breaks when:** an administrator makes a change directly in the target and nobody ever knows. Your entitlement data is fiction, and your certifications certify that fiction.
**Instead:** reconcile every connected system. Report-only first, then remediate on high risk.

### 13. Certification of everything, quarterly

**Tempting because:** it looks maximally rigorous and is the easiest thing to promise an auditor.
**Breaks when:** reviewers face thousands of unintelligible items and click approve-all — producing evidence of a control that didn't operate.
**Instead:** risk-scoped campaigns, business-language descriptions, usage context, and mechanisms ([expiry](../02-identity-fundamentals/15-identity-governance.md), mover reviews) that shrink what needs certifying at all.

### 14. Access request as a ticket form

**Tempting because:** the service desk tool already exists and integrating properly takes months.
**Breaks when:** there's no policy check, no entitlement catalogue, no reconciliation, and no evidence — just a free-text request and someone's best guess at fulfilment.
**Instead:** a real catalogue with owners, policy checks and closed-loop fulfilment. Ticketing may still *execute* the change; it must not *be* the governance.

### 15. Emergency access with no expiry

**Tempting because:** it's an emergency, and cleanup is tomorrow's problem.
**Breaks when:** tomorrow never comes. Emergency grants are permanent grants with an exciting origin story.
**Instead:** technically enforced expiry on every emergency grant, plus mandatory post-hoc review.

---

## Programme and organisation

### 16. The two-year role model project

**Tempting because:** doing roles "properly" genuinely does require broad analysis.
**Breaks when:** month 18 arrives with nothing delivered, the sponsor changes, and the organisation has reorganised twice since the modelling started.
**Instead:** deliver leaver automation and certification first (visible risk reduction), then model roles incrementally using the data you've collected.

### 17. Buying the platform before defining the process

**Tempting because:** an audit finding demands action, and a purchase order is a demonstrable action.
**Breaks when:** the tool's opinion becomes your process by default, and it doesn't fit your organisation.
**Instead:** define the target process first, then select a tool that fits it. Keep requirements tool-neutral. See [choosing a platform](../05-platform-landscape/07-choosing-a-platform.md).

### 18. No operating model

**Tempting because:** the project budget covers implementation, and run costs come from a different pot nobody wants to open.
**Breaks when:** go-live plus 12 months: nobody clears the exception queue, no new applications are onboarded, the role model decays, and the platform becomes an expensive report generator.
**Instead:** design and fund the run phase as part of the architecture ([operating model](../06-business-and-risk/05-operating-model.md)).

### 19. Boiling the ocean

**Tempting because:** partial coverage feels like partial security, and stakeholders ask for a complete plan.
**Breaks when:** nothing ships for two years and the programme is cancelled at the budget review.
**Instead:** risk-rank ruthlessly; make every increment independently valuable.

### 20. The IAM team as a bottleneck

**Tempting because:** central review genuinely catches mistakes, and standards matter.
**Breaks when:** teams route around you — building their own auth, buying their own SaaS, creating their own cloud roles. You now have less control than if you'd been permissive.
**Instead:** make the paved road faster than the alternative. Self-service with guardrails, golden paths, policy in the pipeline. **Influence beats gatekeeping**, and it's the only thing that scales.

---

{: .architect }
> **The meta-pattern:** almost every anti-pattern here is *the fastest way to close today's ticket*. That's precisely why they survive — the person who introduces one is being responsive and helpful. So the architect's job isn't to forbid them; it's to **make the correct path fast enough that the shortcut stops being attractive**, and to insist that where a shortcut is genuinely necessary, it carries an owner and an expiry date. A documented, time-boxed exception is engineering. An undocumented permanent one is debt with compound interest.

---

**Next:** [Diagrams, ADRs & Documentation](08-documentation-and-adrs.md) →

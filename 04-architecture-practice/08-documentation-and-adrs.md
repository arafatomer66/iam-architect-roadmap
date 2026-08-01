---
title: Diagrams, ADRs & Documentation
parent: 4. Architecture Practice
nav_order: 8
---

# Diagrams, ADRs & Documentation

## Why this is a core skill, not an afterthought

An architect's output is **decisions communicated well enough to be implemented and defended.** A design that exists only in your head influences nothing after you leave the meeting; a design nobody can follow gets implemented as whatever the engineer assumed.

{: .concept }
> **Write for the person who inherits your architecture in three years.** They will not have you to ask. They will find a decision that looks wrong, and they will either undo it — losing whatever it protected against — or preserve it in fear, unable to change anything nearby. The single most valuable artefact you produce is not the diagram; it's the **record of why**. Diagrams describe the *what*, and the what is usually reconstructable from the running system. The why is not.

---

## Architecture Decision Records

One decision, one short document, immutable once accepted. A new decision that supersedes it gets a new record.

```markdown
# ADR-014: Use pairwise persistent identifiers for CIAM federation

**Status:** Accepted · 2026-06-12 · Supersedes ADR-009
**Deciders:** IAM architecture, CIAM product, DPO
**Context**

Our CIAM platform federates to 40+ relying parties, including third parties
outside our corporate group. Today we release email address as the NameID.
Legal review flagged that this allows RPs to correlate the same individual
across services without consent, and that email changes have already caused
17 orphaned-account incidents in the last 12 months.

**Decision**

Release a pairwise persistent identifier (unique per RP) as the subject
identifier for all new federations. Email is released as a separate,
consent-gated attribute where the RP has a documented need.

**Consequences**

+ RPs cannot correlate users between themselves.
+ Email changes no longer break account continuity.
+ Reduced personal data release; supports data minimisation.
− Existing 40 RPs must migrate; each requires a mapping exercise and a
  coordinated cutover. Estimated 2 quarters.
− Support cannot identify a user in an RP by their email address; a lookup
  tool is required. (Tracked: IAM-2291.)
− Two RPs contractually require email as the identifier. These remain on
  the old scheme as documented exceptions, reviewed annually.

**Alternatives considered**

1. *Keep email as NameID.* Rejected: does not address either problem.
2. *Global persistent pseudonym (same value to all RPs).* Rejected: fixes
   the stability problem but not correlation; legal considered it insufficient.
3. *Transient identifiers.* Rejected: RPs need to recognise returning users.
```

**What makes an ADR good:**

- **The context is honest**, including the political and organisational constraints. "The vendor contract runs until 2028" is legitimate context.
- **Consequences include the negatives.** An ADR with only benefits is marketing, and reviewers discount it accordingly.
- **Rejected alternatives are recorded with reasons.** This is the part that saves your successor from re-litigating a settled question — and saves you from re-litigating it in six months when a new stakeholder arrives.
- **It's short.** One or two pages. Long ADRs don't get read, and unread records provide no value.
- **It's stored with the code or in a versioned repository**, not in an attachment on a wiki page nobody can find.

Write one for: choosing a product, choosing a protocol where a choice exists, a token lifetime, an authoritative source, a trust boundary, a deviation from standard, an accepted risk, and anything you expect to be questioned. **If you can't name the alternatives you rejected, it isn't a decision — it's a default**, and defaults deserve to be identified as such.

---

## Diagrams that earn their place

Different audiences need different altitudes. The **C4** model is a useful ladder:

| Level | Shows | Audience |
|:--|:--|:--|
| **Context** | Your system and the things around it | Executives, stakeholders |
| **Container** | Deployable units and how they talk | Architects, senior engineers |
| **Component** | Inside one container | Implementing engineers |
| **Code** | Classes, schemas | Rarely worth drawing |

IAM-specific diagrams that consistently earn their keep:

**The identity flow diagram** — where identity data originates, how it's transformed and where it lands. Answers "where does this attribute come from?" for everyone, permanently.

**The authentication sequence** — one per pattern (workforce SSO, CIAM login, service-to-service, legacy proxy). Sequence diagrams, not boxes; the ordering *is* the content.

**The trust boundary diagram** — who trusts whom to assert what. Underrated and clarifying, because it exposes assumptions nobody has stated.

**The lifecycle state machine** — pre-hire → active → leave → notice → terminated → archived, with the triggers and what happens on each transition. Resolves more arguments than any other single artefact.

**The application coverage map** — every application, its authentication method, provisioning method, governance level and risk tier. Usually a table rather than a picture, and it's the artefact stakeholders reference most.

{: .architect }
> **Diagram-as-code (Mermaid, PlantUML, Structurizr) beats drawing tools for architecture.** It diffs in review, it lives with the design document, it can't be edited by someone who then loses the source file, and it forces a level of abstraction that stops people adding decorative detail. The most common reason architecture diagrams go stale is that only one person has the editable file — a problem that disappears entirely when the diagram is text in a repository.

---

## The design document

For anything substantial, a document that says:

1. **Problem and drivers** — what happens if we do nothing, and why now.
2. **Constraints** — stated up front ([architectural thinking](01-architectural-thinking.md)).
3. **Requirements**, functional and quality-attribute, with numbers and owners.
4. **Current state**, honestly.
5. **Target state**, as capabilities, with diagrams.
6. **Gap and sequence** — increments, each independently valuable.
7. **Key decisions**, linking to ADRs.
8. **Risks and assumptions**, with owners.
9. **Operating model** — who runs it, at what cost.
10. **What we are explicitly *not* doing**, and why.

That last section is disproportionately valuable. Out-of-scope declarations prevent both scope creep and the accusation of oversight, and they surface disagreement early — which is exactly when you want it.

---

## Standards and patterns documents

Beyond individual designs, an architect publishes the rules others build to:

- **Application onboarding standard** — what an application must support to be onboarded, at each risk tier.
- **Token and session standard** — lifetimes, algorithms, storage rules.
- **Naming conventions** — groups, roles, service accounts. Boring, and they prevent years of chaos.
- **Integration patterns** — the sanctioned ways to connect, with examples.
- **Definition of "privileged"** — so the term means one thing.
- **Exception process** — how to deviate, who approves, how it expires.

Standards should be **short, opinionated and enforceable**. A 60-page standard is not read; a two-page standard with a checklist is applied. Where possible, encode the standard in tooling (pipeline checks, templates, golden paths) so compliance is the path of least resistance rather than an act of virtue.

---

## Keeping documentation alive

Documentation rots. Countermeasures that work:

- **Keep it with the thing it describes.** Design docs and diagrams in the same repository as the configuration.
- **Update as part of the change**, not as a follow-up task. A change that doesn't update the diagram isn't complete.
- **Prefer generated over hand-maintained** where possible — application inventories, connector lists and coverage maps should come from the platform, not from someone's spreadsheet.
- **Date and own everything.** An undated document of unknown provenance will be distrusted and then ignored — which is the correct response.
- **Delete aggressively.** Wrong documentation is worse than none, because people act on it.

---

## Architect's checklist

- [ ] Is there an **ADR for every significant decision**, including rejected alternatives?
- [ ] Do your ADRs record **negative consequences** honestly?
- [ ] Are diagrams **diagram-as-code**, versioned alongside the design?
- [ ] Do you have the five IAM-specific diagrams: identity flow, authentication sequences, trust boundaries, lifecycle state machine, application coverage map?
- [ ] Does every design document state **what is explicitly out of scope**?
- [ ] Are constraints and quality attributes recorded **with numbers and owners**?
- [ ] Are your standards **short enough to be read** and encoded in tooling where possible?
- [ ] Is the **exception process** documented, with expiry built in?
- [ ] Is documentation stored where an engineer will actually find it?
- [ ] Would someone inheriting this estate in three years understand **why**, not just what?

---

**Next:** [Stage 5 — Platform Landscape](../05-platform-landscape/) →

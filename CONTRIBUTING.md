# Contributing

This is an educational repo. Contributions that make it more accurate, clearer, or more useful to someone learning IAM architecture are very welcome.

## What fits

- **Corrections.** Protocol details, standards references, terminology. If something here is wrong, say so — ideally with a link to the spec or vendor doc.
- **Missing concepts.** If an architect needs to know it and it isn't here, open an issue describing the gap.
- **Better explanations.** Shorter, clearer, better-illustrated versions of existing material.
- **Case studies and scenarios.** Anonymised, generalised, no confidential detail.
- **Diagrams.** Mermaid only (renders on GitHub *and* on the site). No image files unless there's no alternative.

## What doesn't fit

- **Product marketing.** Vendor sections describe *how a product expresses a concept*, in neutral language. No "leader in the Magic Quadrant" content, no feature-list dumps, no comparison tables that pick a winner.
- **Anything under NDA**, or drawn from a specific employer's internal architecture.
- **Screenshots of product consoles.** They go stale in months and carry licensing questions.
- **Step-by-step product configuration.** That's vendor documentation's job, and it rots. Teach the decision, link the doc.
- **Offensive tooling or exploit code.** Attack *concepts* (Kerberoasting, golden tickets, consent phishing, token replay) belong here because architects must design against them. Working exploits do not.

## House style

- **Concept → mechanics → architect's lens.** Every substantive page follows that order.
- **Explain why the thing exists** before explaining how it works. A protocol without its problem is trivia.
- Vendor mentions in concept pages go inside an `{: .vendor }` callout so they're visually separable from the durable content.
- Every Stage 1–8 page ends with an **Architect's checklist** — questions the reader should be able to answer, not a summary.
- Use British or American spelling consistently within a page; don't mix.
- Prefer tables over long lists when comparing things. Prefer prose over bullets when explaining reasoning.
- Keep pages self-contained enough to be read alone, and cross-link liberally with relative `.md` links (they work on GitHub and are rewritten by Jekyll for the site).

## Front matter

Every page needs it, or it won't appear in the site navigation:

```yaml
---
title: Human Readable Title
parent: 2. Identity Fundamentals   # exact title of the section index page
nav_order: 7
---
```

Section index pages use `has_children: true` and no `parent`.

## Process

1. Open an issue first for anything larger than a fix — it saves you writing a page that duplicates one already drafted.
2. One topic per PR.
3. Check that relative links resolve and that any Mermaid renders on GitHub before pushing.

By contributing you agree your contribution is licensed under [CC BY 4.0](LICENSE).

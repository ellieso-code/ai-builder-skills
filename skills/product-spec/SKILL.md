---
name: product-spec
description: Use when starting a new coding project and need a one-page working spec before writing code, when the user types /product-spec, or as part of a kickoff orchestrator skill. For working specs (solo builders), not stakeholder PRDs.
---

# Product Spec

## Overview

Walks a user through writing a one-page product spec for a solo coding project. The output is a working document the agent uses to plan and build against — not a stakeholder PRD. Optimized for solo builders shipping personal, learning, or MVP projects.

The skill uses a walkthrough-with-confirmation pattern: read what the user has already provided (initial brief, brainstorm output, or earlier answers), walk the 7 sections in order, propose an interpretation for each section, confirm before advancing. Do not silently fill sections from the brief.

## When to Use

- Starting a new coding project and need a spec before planning
- User typed `/product-spec`
- Invoked from a kickoff orchestrator skill
- Want to capture project intent in a way the agent can verify against later

## When NOT to Use

- Existing project with scope already documented — use `superpowers:writing-plans` directly
- Enterprise or customer-facing feature needing a full PRD with risk matrix, personas, KPIs — use a heavier PRD tool instead
- Trivial change, single-file edit, or bug fix — over-ceremony

## The 7 Sections

1. **What** — 1 sentence describing the thing being built
2. **Why** — 1 sentence on motivation
3. **Who it's for** — 1–2 sentences, optionally with user inputs
4. **Scope** — 3–5 items, each with a specific acceptance criterion
5. **Success criteria** — 2–4 testable bullets
6. **Non-goals** — 2–4 bullets (hard guardrail against scope drift)
7. **Constraints** — tech stack, time budget, design guidelines, linked API docs

## The Walkthrough Pattern

For each section, in order:

1. Read what the user has already provided in the brief, brainstorm output, or earlier answers
2. Draft a proposed version of the section
3. Show it to the user with: "Here's what I have in mind for [section]: [draft]. Right? Anything to add or change?"
4. Wait for confirmation, edit, or "anything else?"
5. If the user adds something, incorporate and re-confirm
6. Advance to the next section

Never silently fill a section from the brief. Always surface the interpretation. Silent fills are the most common way the agent and user diverge.

## Output

**Path detection:**
- If a `product/` folder exists in the current directory → write to `product/SPEC.md`
- Otherwise → write to `SPEC.md` at the project root

**Format:** Markdown, h2 per section, header with project name and date.

**AI-feature flag:** If any section mentions an AI feature (Claude API, OpenAI, LLM call, embeddings, generation, etc.), add a `## Notes` section with this exact line: `> AI feature flag — generate prompt.md in kickoff`. A kickoff orchestrator scans for this line and triggers a prompt.md scaffolder. Skip if no AI feature.

**After save:** confirm the path written. Suggest the next step:
- If invoked from a kickoff orchestrator → return control to it
- If standalone → suggest `superpowers:writing-plans` to break scope into PLAN.md phases

## Template

```markdown
# [Project Name] — Spec

**Date:** YYYY-MM-DD
**Status:** Draft

## What
[1 sentence]

## Why
[1 sentence]

## Who it's for
[1–2 sentences. Optional: list user inputs, e.g., city, age, range]

## Scope
1. [Item] — done means: [specific, checkable criterion]
2. [Item] — done means: [specific, checkable criterion]
3. [Item] — done means: [specific, checkable criterion]

## Success criteria
- [Testable bullet]
- [Testable bullet]

## Non-goals
- [Specific thing not in v1]
- [Specific thing not in v1]

## Constraints
- Tech stack: [e.g., React + Express + Claude API]
- Time budget: [e.g., MVP this weekend]
- Design: [e.g., minimal, no animations]
- API docs: [link to relevant external docs]

## Notes
[Optional. If "What" describes an AI feature, include: `> AI feature flag — generate prompt.md in kickoff`]
```

## Conventions

- Filename: `SPEC.md` (uppercase, matching `PLAN.md` and `TODO.md`)
- Date format: YYYY-MM-DD
- Whole spec fits on one page when viewed in a markdown renderer
- Acceptance criteria are checkable, not aspirational
- Non-goals are specific things, not vague qualities

## Common Mistakes

- **Silently filling sections from the brief.** Surface every interpretation. Solo builders skip these confirmations and then discover the agent built the wrong thing.
- **Aspirational success criteria.** "Easy to use" is not testable. "I can log an item in under 10 seconds" is.
- **Vague non-goals.** "No bloat" is not a guardrail. "No auth in v1, no mobile app, no offline sync" is.
- **Putting milestones in the spec.** Milestones belong in PLAN.md. Scope just lists what's in.
- **Putting tech stack in Scope.** Tech stack lives in Constraints. Scope is what; Constraints is with-what.
- **Skipping non-goals.** This section is the highest-leverage anti-slop tool. The agent uses it as a hard guardrail. Never empty.

## Hand-off

After the spec is saved:

- If a kickoff orchestrator invoked this skill, return control to it for the next steps (CLAUDE.md scaffolder, PLAN.md, TODO.md, etc.)
- If invoked standalone, suggest the user run `superpowers:writing-plans` to break the spec's scope into a phased PLAN.md

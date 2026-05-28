# product-spec skill

One-page working spec for solo coding projects. Walks you through 7 sections one at a time, proposing interpretations from your brief before silently filling them. Output is a `SPEC.md` the agent plans and builds against.

## What problem this solves

Specs at companies are heavyweight PRDs designed for stakeholder alignment. Solo builders don't need that — they need a tight document the agent can verify against. This skill produces that: a one-page spec with the 7 highest-leverage sections, explicit non-goals as the anti-slop guardrail, and acceptance criteria the agent can check at the end of every phase.

Designed for builders shipping personal, learning, or MVP projects, not stakeholder PRDs.

## Installation

Install this skill alone:

```bash
cp -r skills/product-spec ~/.claude/skills/
```

Or install the full kickstart bundle (recommended — see `build-kickoff` README):

```bash
cp -r skills/build-kickoff skills/product-spec skills/design-md ~/.claude/skills/
```

Claude Code picks up new skills on the next session start.

## How to use this skill

Type any of these to trigger it:

- `/product-spec`
- "Create a spec for my project"
- "Write a SPEC.md for [X]"

It's also invoked automatically as step 4 of [`/build-kickoff`](../build-kickoff/).

## The 7 sections (in order)

1. **What** — 1 sentence describing the thing
2. **Why** — 1 sentence motivation
3. **Who it's for** — 1-2 sentences, optionally with user inputs
4. **Scope** — 3-5 items with acceptance criteria
5. **Success criteria** — 2-4 testable bullets
6. **Non-goals** — 2-4 explicit "not building" bullets (the hard guardrail)
7. **Constraints** — tech stack, time budget, design guidelines, linked API docs

## The walkthrough pattern

The skill reads any brief or context you've provided, then walks the 7 sections one at a time. For each section, it:

1. Proposes a draft based on what you've shared
2. Shows it to you: "Here's what I have in mind for [section]: [draft]. Right? Anything to add?"
3. Waits for confirmation, edit, or "anything else?"
4. Advances to the next section

It never silently fills a section from the brief. Always surfaces the interpretation. This catches misalignments before they poison the downstream plan.

## What you get

A markdown file at:
- `product/SPEC.md` (if a `product/` folder exists in your project)
- `SPEC.md` at project root (otherwise)

Containing all 7 sections, with:
- Explicit non-goals (the highest-leverage anti-slop section)
- Acceptance criteria per scope item
- Optional AI-feature flag line (picked up by `build-kickoff` for `prompt.md` scaffolding)

## Key design choices

1. **Order matters.** What → Why → Who → Scope → Success → Non-goals → Constraints. Scope comes before Success because the agent needs to know what you're building before it can verify done. Non-goals come right after Success because they're the guardrail the agent uses to push back on scope drift.

2. **One page total.** All 7 sections fit on a single page in a markdown renderer. Specs longer than that are stakeholder PRDs, not solo-builder working docs.

3. **Acceptance criteria, not aspirational claims.** Each scope item ends with "done means: [checkable thing]." Each success criterion is testable. "Easy to use" is banned. "I can log an expense in under 10 seconds" is fine.

4. **Walkthrough with confirmation, not pure Q&A or pure dump.** Reads your brief, proposes interpretations, asks you to confirm. Tighter than Q&A, less error-prone than silently filling from a brain dump.

5. **AI-feature flag as a downstream signal.** If your spec mentions an AI feature (Claude API, OpenAI, LLM call, etc.), the skill adds a flag line that `build-kickoff` picks up to generate a `prompt.md` artifact alongside the spec.

## When to use this skill

**Good fits:**
- Starting a new coding project and need a spec before planning
- Want a one-page working document, not a stakeholder PRD
- Running it as step 4 of `/build-kickoff`

**Not a fit:**
- Adding a feature to an existing project (use brainstorming + writing-plans directly)
- Trivial change, single-file edit, or bug fix (over-ceremony)
- Customer-facing feature with stakeholders, KPIs, risk matrix — use a heavier PRD tool

## Composition with other skills

This skill is part of the **build-kickoff bundle** in this repo. It composes with:

- **[`build-kickoff`](../build-kickoff/)** — orchestrator that invokes `product-spec` at step 4, then continues with design system, plan, constitution, and review gate
- **[`design-md`](../design-md/)** — sibling skill that creates the design system file (`DESIGN.md`) for UI projects
- **[`superpowers:writing-plans`](https://github.com/obra/superpowers)** — recommended follow-on if running `product-spec` standalone; turns the spec's scope into a phased `PLAN.md`

## Files in this folder

| File | Audience | Purpose |
|------|----------|---------|
| `SKILL.md` | The AI agent | Full walkthrough pattern with all 7 sections and common mistakes |
| `README.md` | Humans | This file: quick reference for whoever invokes or shares the skill |

## Version

Built 2026-05-28.

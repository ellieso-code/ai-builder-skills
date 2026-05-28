---
name: build-kickoff
description: Use when starting a new code project from scratch, the user types /build-kickoff, or asks to kickstart/start/begin a new app or project. For greenfield only — adding features to existing projects use brainstorming + writing-plans directly.
---

# Build Kickoff

## Overview

Orchestrates the kickoff phase of a new code project. Walks from vague idea to reviewed plan, ready for the build phase. Greenfield only.

Invokes other skills (`product-spec`, `design-md`, `superpowers:writing-plans`) and generates a few artifacts inline (`CLAUDE.md`, `TODO.md`, optionally `prompt.md`). Each step has an explicit confirmation gate. No code is written until the user approves the full plan.

Core principle: maximize the kickoff phase to minimize iteration churn. A tight spec + design + plan + constitution at the start saves hours of mid-build scope drift and prevents the generic "AI slop" aesthetic.

**Dependencies (skill bundle):** `product-spec`, `design-md`, `superpowers:writing-plans`, `superpowers:executing-plans`.

## When to Use

- Starting a brand new code project from scratch
- User typed `/build-kickoff`
- User said "let's start a new project," "kickstart X," or "build a new app"

## When NOT to Use

- Adding a feature to an existing project → use `superpowers:brainstorming` + `superpowers:writing-plans` directly
- Single-file edit, bug fix, or refactor → other skills
- Quick exploratory hack or one-off script → over-ceremony

## The 12-Step Flow

### 1. Brief intake

Ask the user: "Tell me what you want to build — what, why, who's it for? Just dump it all, messy is fine."

Capture the response. Derive a project slug (kebab-case from the project name).

**Slug derivation fallback:** if the brief doesn't contain a clear project name, ask the user: "What should I call this project? (kebab-case, e.g., `expense-tracker`)"

Pass the brief verbatim and the slug as context to `product-spec` at step 4. Detailed section-by-section walkthrough happens at step 4, not here.

### 2. Conditional folder check

Detect if the current working directory looks project-specific:
- Folder name matches the slug
- Has project markers (`.git`, `package.json`, `pyproject.toml`, `CLAUDE.md`, `README.md`)
- Is sparse (few files, or empty)

If yes, continue silently. If no, ask the user where to work. Default suggestion: `<current-dir>/<slug>/`. Create the folder if missing and `cd` into it.

### 3. Solution exploration (Plan Mode)

Prompt the user to enter Plan Mode (shift+tab). Explain: "Plan Mode keeps the next step read-only so we explore without committing."

Generate 3 solution approaches, ordered simplest → fanciest. For each:
- One-line description
- Tech stack implications
- Key trade-off (simplicity vs. capability)

Only present alternatives if the project specs would genuinely fit a different stack — don't challenge the obvious default for the sake of challenge.

Ask the user to pick one. Then exit Plan Mode:
- If `ExitPlanMode` tool is available, call it directly.
- Otherwise prompt the user: "Exit Plan Mode (shift+tab) — step 4 needs to write files."

**Verify Plan Mode is off before step 4.** If a write fails at step 4 because Plan Mode is still on, re-prompt the user to toggle off and retry.

### 4. Product spec

Invoke `product-spec`. Pass the brief and chosen solution as context. The skill walks 7 sections and writes `SPEC.md` (or `product/SPEC.md` if `product/` exists).

After the spec is written, **auto-detect two flags** by scanning the spec content:

- **AI-feature flag** (triggers step 9):
  - Tech stack mentions: "Claude API", "OpenAI", "Anthropic", "Gemini", "GPT", "LLM"
  - Scope items mentioning generation, prompting, embeddings, model calls
  - API doc links to Anthropic, OpenAI, etc.

- **UI-project flag** (triggers step 5):
  - Tech stack mentions: "React", "Vue", "Next.js", "Svelte", "HTML", "CSS", "Tailwind", "frontend", "web app", "mobile app"
  - Scope items mentioning: "screen", "page", "component", "form", "view", "interface", "display"

### 5. (Conditional) DESIGN.md

If the UI-project flag is set, frame the choice for the user:

> "Step 5: Design system. Do you have screenshots, links to apps you like, or brand colors you want to seed this with? If yes, share them now. If not, `design-md` will walk you through choosing from scratch."

Then:

- **If the user shares references or wants to proceed:** invoke `design-md`. The skill walks through colors, typography, spacing, components, dos and don'ts. References (if any) seed the system.
- **If the user explicitly asks to skip** (e.g., "I have no idea, can we skip this for now?"): accept and skip. Note they can return to `design-md` later. Do not push.

Don't proactively offer to skip — only honor explicit skip requests. The default is to proceed because this is the highest-leverage step for avoiding the generic AI aesthetic ("slop look"): without an explicit design system, agents default to Inter + purple gradients + shadcn components on every generation.

If no UI detected, skip this step entirely.

### 6. CLAUDE.md scaffolder

Generate `CLAUDE.md` at the project root containing:

- **Project name and one-line purpose** (from spec)
- **Tech stack** (from spec Constraints)
- **Design system reference** (only if `DESIGN.md` exists): "Use design tokens from `DESIGN.md` for all UI work. Do not default to Inter, shadcn, or generic gradients."
- **Behavioral guardrails** (Karpathy guidelines):
  - Think before coding: state assumptions, ask when uncertain
  - Simplicity first: no abstractions for single-use code, no flexibility not requested, no error handling for impossible scenarios
  - Surgical changes: touch only what you must, clean up only your own mess
  - Goal-driven: every change should trace to a stated requirement
- **Test policy** (ask the user: required per task, or only at phase boundaries?)
- **Commit policy** (descriptive commits, no co-author tags unless explicitly requested)

Show the user the draft before writing.

### 7. PLAN.md

Invoke `superpowers:writing-plans`, passing this hint: "Default phase 1 = UI with dummy data. Default phase 2 = connect real APIs / data. Adjust to project needs. Phases are vertical slices, 3-5 total, each with a 1-line definition of done."

Output goes to the same location convention as the spec (`PLAN.md` at root or `product/PLAN.md` if `product/` exists).

### 8. TODO.md scaffolder

Generate `TODO.md` next to PLAN.md:

```
# TODO

## In Progress
- [ ] (none)

## Next
[Tasks from PLAN.md phase 1, sized 10-15 min each, each naming the file + acceptance check]

## Done
```

The agent owns this file during execution. The user reviews at phase boundaries. Completed items move to `## Done` with a date stamp (not deleted — it's a learning log).

### 9. (Conditional) prompt.md scaffolder

If step 4 auto-detected an AI feature, generate `prompt.md` next to PLAN.md:

```
# Prompt — [feature name]

## Goal
[1 sentence — what should the AI call produce?]

## Inputs
[Listed from spec's Who-it's-for inputs]

## Prompt template
[Skeleton with input placeholders]

## API
- Provider: [Claude / OpenAI / etc.]
- Model: [task-appropriate model]
- Tools: [tools needed, e.g., web search]
- Docs: [link to provider docs]
```

If no AI feature detected, skip this step.

### 10. Plan review gate

**STOP.** Display all generated artifacts with their paths:
- `SPEC.md` (or `product/SPEC.md`)
- `DESIGN.md` (if applicable)
- `CLAUDE.md`
- `PLAN.md` (or `product/PLAN.md`)
- `TODO.md` (or `product/TODO.md`)
- `prompt.md` (if applicable)

Ask the user: "Review these. Approve to continue, or tell me what to change."

Do not proceed until they approve. If they request changes, return to the relevant step.

### 11. GitHub repo link

Ask the user to create a GitHub repo and paste the URL. Confirm the URL is reachable (`git ls-remote` or similar). Suggest the first commit message: "Initial kickoff: spec + plan + constitution."

If the user prefers to skip GitHub for now, skip and note they can return to it later.

### 12. Hand-off

Ask the user: "Kickoff complete. Ready to start building?"

- **If yes:** invoke `superpowers:executing-plans` to begin the build phase.
- **If no:** state "Pausing here. When ready, run `superpowers:executing-plans`. Plan is at `PLAN.md`." Stop.

## Common Mistakes

- **Skipping the folder check at step 2.** Files scatter across the workspace. Never skip.
- **Forgetting to prompt for Plan Mode exit at step 3.** Plan Mode prevents file writes. Step 4 will fail if you don't prompt the exit.
- **Challenging the default stack for the sake of challenge.** Only surface alternatives at step 3 when the project specs genuinely fit a different stack.
- **Proactively offering to skip DESIGN.md.** Default is to proceed. Only honor explicit skip requests from the user — never offer skipping upfront. Without an explicit design system, UI projects default to the generic AI aesthetic.
- **Pushing the user through DESIGN.md when they explicitly ask to skip.** Respect explicit skip requests. They can return to `design-md` later.
- **Skipping the plan review gate at step 10.** This is the highest-leverage gate. Never skip even if the user seems eager.
- **Invoking brainstorming or any other skill that itself ends in writing-plans.** Build-kickoff calls writing-plans itself at step 7. Sub-skill duplicate invocations cause two plan attempts.
- **Auto-invoking executing-plans at step 12 without asking.** The "ready?" question is the contract — respect it.

# build-kickoff skill

Greenfield orchestrator for starting a new code project the right way. Walks 12 steps from vague idea to reviewed plan, ready for the build phase. Designed for solo builders shipping personal, learning, or MVP projects with AI coding agents.

## What problem this solves

Most new projects start with a vague prompt ("build me an X"), which leaves the agent inventing scope, defaulting to generic stacks, and shipping the same "AI slop" aesthetic every time. Build-kickoff replaces that with a structured kickoff:

```
brief → folder → solution exploration → spec → design system →
constitution → phased plan → todo → review gate → hand-off
```

A tight spec, design system, plan, and CLAUDE.md at the start save hours of mid-build scope drift and prevent the "looks like every AI app" aesthetic.

Built from the 2026 consensus on spec-driven development with Claude Code (Anthropic best practices, Karpathy behavioral guardrails, Spec Kit-style artifacts) without the BMAD ceremony.

## Installation

This skill invokes two other skills from this repo plus two from the [superpowers plugin](https://github.com/obra/superpowers). Install all of them together so `/build-kickoff` works end-to-end.

**Step 1: Install the three bundle skills from this repo:**

```bash
cp -r skills/build-kickoff skills/product-spec skills/design-md ~/.claude/skills/
```

**Step 2: Install the [superpowers plugin](https://github.com/obra/superpowers)** for Claude Code (provides `writing-plans` and `executing-plans`):

```
/plugin install superpowers@claude-plugins-official
```

The [superpowers plugin](https://github.com/obra/superpowers) is a community plugin for Claude Code that adds spec-driven workflow skills (brainstorming, writing-plans, executing-plans, TDD, code review, and more). It's the most widely-used skill toolkit for spec-driven development with Claude Code in 2026. `build-kickoff` invokes two of its skills directly.

Claude Code picks up new skills on the next session start.

If you skip `product-spec` or `design-md`, `/build-kickoff` will fail at step 4 (no spec writer) or step 5 (no design system writer) respectively. If you skip the superpowers plugin, it'll fail at step 7 (no plan writer) and step 12 (no executing-plans for hand-off).

## How to use this skill

Type any of these to trigger it:

- `/build-kickoff`
- "Let's start a new project"
- "Kickstart a [X]"
- "Build me a new [app / CLI / tool]"

## The 12-step flow

| Step | What | Artifact |
|------|------|----------|
| 1 | Brief intake | (project slug derived) |
| 2 | Conditional folder check | (project folder created if needed) |
| 3 | Solution exploration (in Plan Mode) | (one approach picked) |
| 4 | Product spec → invokes `product-spec` | `SPEC.md` |
| 5 | (Conditional) DESIGN.md → invokes `design-md` if UI project | `DESIGN.md` |
| 6 | CLAUDE.md scaffolder (inline) | `CLAUDE.md` |
| 7 | PLAN.md → invokes `superpowers:writing-plans` | `PLAN.md` |
| 8 | TODO.md scaffolder (inline) | `TODO.md` |
| 9 | (Conditional) prompt.md scaffolder if AI feature flagged | `prompt.md` |
| 10 | **Plan review gate** — STOP for user approval | — |
| 11 | GitHub repo link | (repo connected) |
| 12 | Hand-off — asks "ready to build?" | (passes to `superpowers:executing-plans`) |

## What you get

A new project folder containing:

- `SPEC.md` — one-page product spec (7 sections)
- `DESIGN.md` — design system (if it's a UI project)
- `CLAUDE.md` — project constitution with behavioral guardrails
- `PLAN.md` — phased plan (3–5 vertical slices)
- `TODO.md` — agent-owned task list (In Progress / Next / Done)
- `prompt.md` — AI prompt template (if AI feature detected)

Plus a GitHub repo linked, and the agent primed to start executing the plan.

## Dependencies

| Skill | Where to get it |
|-------|-----------------|
| `product-spec` | This repo: [`skills/product-spec/`](../product-spec/) |
| `design-md` | This repo: [`skills/design-md/`](../design-md/) |
| `superpowers:writing-plans` | [superpowers plugin](https://github.com/obra/superpowers) |
| `superpowers:executing-plans` | [superpowers plugin](https://github.com/obra/superpowers) |

See the Installation section above for the full setup.

## Key design choices

A few decisions in the skill worth knowing about:

1. **Greenfield only.** This skill is for *starting* new projects. For adding features to existing projects, use `superpowers:brainstorming` + `superpowers:writing-plans` directly.

2. **Plan Mode for one step only.** Solution exploration (step 3) runs in Plan Mode for the deliberation benefit. Every other step runs with Plan Mode off because the orchestrator has its own review gates.

3. **Auto-detection for AI features and UI projects.** The skill scans the spec content for keywords (Claude API, OpenAI, React, Vue, etc.) to decide whether to trigger `prompt.md` and `DESIGN.md` scaffolders. No explicit asks for these flags.

4. **Skip-if-asked for DESIGN.md.** The default is to walk through the design system because it's the #1 anti-slop tool for UI projects. But if you explicitly say "I have no idea, skip this," the skill respects that. You can return to `design-md` later.

5. **Manual hand-off at step 12.** The skill asks "ready to start building?" instead of auto-invoking `executing-plans`. The breakpoint between planning and building is intentional — it lets you take a breath, come back later, or split the work across sessions.

6. **No challenge for challenge's sake.** Step 3 only surfaces alternative stacks if the project specs would genuinely fit a different one. Otherwise the skill takes the obvious default and moves on.

## When to use this skill

**Good fits:**
- Starting a new code project from scratch (web app, CLI, mobile app, library)
- Want a structured spec + design + plan + constitution before any code
- Want to maximize the kickoff phase so iterations stay tight
- Want to avoid the generic AI aesthetic by locking in a design system before writing UI

**Not a fit:**
- Adding a feature to an existing project (use brainstorming + writing-plans)
- Bug fixes or single-file edits (over-ceremony)
- Quick exploratory hacks or one-off scripts (over-ceremony)

## Files in this folder

| File | Audience | Purpose |
|------|----------|---------|
| `SKILL.md` | The AI agent | Full 12-step flow with all instructions and common mistakes |
| `README.md` | Humans | This file: quick reference for whoever invokes or shares the skill |

## Version

Built 2026-05-28.

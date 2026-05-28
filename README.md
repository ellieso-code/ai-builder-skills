# ai-builder-skills

A series of tools for AI builders using coding agents like Claude Code.

This repo is a collection. Each skill lives in its own folder under `skills/`, with a `SKILL.md` file that defines its trigger, scope, and behavior.

## What this is

A library of skill files (`SKILL.md` + supporting assets) you can install into Claude Code.

## What this is not

A list of "skills you need to become an AI builder." This repo holds executable instruction files for AI agents, not career advice.

## Installing a skill

Skills live in `~/.claude/skills/<skill-name>/`. To install one from this repo:

```bash
cp -r skills/<skill-name> ~/.claude/skills/
```

Claude Code picks up new skills on the next session start. Invoke a skill with `/<skill-name>` or let Claude trigger it automatically based on the skill's description.

## Skills in this repo

- **[design-md](skills/design-md/)** — Interactive design systems specialist that creates DESIGN.md files in the [Google Labs format](https://github.com/google-labs-code/design.md). Walks you through colors, typography, spacing, components, and dos-and-donts with questions that capture intent, not just values. Useful for PMs and builders who want AI coding tools to generate distinctive UIs instead of generic AI-slop aesthetics.
- **[product-spec](skills/product-spec/)** — One-page working spec for solo coding projects. Walks you through 7 sections (What, Why, Who, Scope, Success criteria, Non-goals, Constraints) one at a time, proposing interpretations from your brief before silently filling them. Output is a SPEC.md the agent can plan and build against. Designed for builders shipping personal, learning, or MVP projects, not stakeholder PRDs.
- **[build-kickoff](skills/build-kickoff/)** — Greenfield orchestrator for kicking off a new code project the right way. Walks from vague idea to reviewed plan: brief → folder check → solution exploration (in Plan Mode) → spec → design system → CLAUDE.md → phased plan → todo list → optional AI prompt → review gate → GitHub → hand-off to executing-plans. Invokes `product-spec` and `design-md` from this repo plus `superpowers:writing-plans` and `superpowers:executing-plans`. Use to maximize the kickoff phase so iterations stay tight and the agent avoids the generic AI slop aesthetic.

## Skill bundle: kickstart a new project

Three skills in this repo compose into a complete kickoff workflow for new code projects:

- `build-kickoff` (the orchestrator)
- `product-spec` (writes the spec)
- `design-md` (writes the design system, conditional on UI projects)

Install all three together to run `/build-kickoff` end-to-end:

```bash
cp -r skills/build-kickoff skills/product-spec skills/design-md ~/.claude/skills/
```

`build-kickoff` also depends on `superpowers:writing-plans` and `superpowers:executing-plans` from the [superpowers plugin](https://github.com/obra/superpowers). Install via `/plugin install superpowers@claude-plugins-official`.

## License

MIT. See [LICENSE](LICENSE).

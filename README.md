# ai-builder-skills

Claude Code skills for product managers and AI builders.

Skills are reusable instruction modules that teach Claude Code (and similar agents) how to do specific tasks well: write a PRD, run a structured brainstorm, audit a design system, ship faster. Drop one into your `~/.claude/skills/` folder and Claude picks it up automatically.

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

## A note on contributions

This is a personal collection, not currently open for outside contributions. Feel free to fork, copy, or adapt any skill for your own use under the MIT license. If you spot a bug or have a strong suggestion, you're welcome to open an issue, but I may not act on it.

## License

MIT. See [LICENSE](LICENSE).

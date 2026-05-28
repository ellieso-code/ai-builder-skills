# design-md skill

Interactive design systems specialist that creates DESIGN.md files in the Google Labs format. Useful for designers who want to capture a system precisely, product managers building with AI coding tools, developers shipping front-end work, and teams that want a single source of truth across humans and agents.

## What is a DESIGN.md file?

A DESIGN.md file is a single markdown document that tells AI coding tools (Claude Code, Cursor, v0, Figma, and any agent that consumes the format) how an app should look. It pairs YAML design tokens (colors, fonts, spacing) with markdown prose (intent, constraints, dos-and-donts).

It's the file that stops AI tools from defaulting to generic "AI slop" UI (Inter font, purple gradients on white, predictable layouts). The format originated at Google Labs and is becoming the cross-tool standard.

Official spec: https://github.com/google-labs-code/design.md

## How to use this skill

Type any of these to trigger it:

- `/design-md`
- "Create a DESIGN.md for my project"
- "Extract a design system from [site/screenshot]"
- "Build design tokens for [app]"
- "Merge these DESIGN.md files"

The skill is named Mira and runs as a guided conversation.

## What it does

The skill supports four modes. The first question determines which one.

| Mode | What it does | Save location |
|------|-------------|---------------|
| **A: Study** | Extract a DESIGN.md from a live site or screenshots, for reference or inspiration | `inspirations/<source>-DESIGN.md` (asks you) |
| **B: Existing app** | Detect your codebase's tokens (Tailwind, CSS variables, theme files), turn them into a DESIGN.md | Project root as `DESIGN.md` |
| **C: New project** | Build from scratch, with or without inspiration references | Current working directory |
| **D: Merge** | Combine multiple DESIGN.md sources into one, with conflict resolution | You choose |

## The flow

1. **Mode selection** (1 question).
2. **Context gathering** (mode-specific): WebFetch for URLs, multimodal analysis for screenshots, config file scanning for existing codebases.
3. **Section-by-section build** in canonical order: Overview, Colors, Typography, Layout, Elevation & Depth, Shapes, Components, Motion (extension), Do's and Don'ts.
4. **Quality gate**: runs the official linter (`npx @google/design.md lint`) to verify YAML structure, contrast ratios, token references, and section order.
5. **Save**, then offer to generate a `tailwind.config.js` or `theme.css` via the CLI's export command.

For each section, the skill asks questions that elicit the *intent* layer, not just the values. Example for colors:

- "What's the primary color for? CTAs only, brand moments, or backgrounds?"
- "Where should the primary color NEVER appear?"

This is the highest-leverage part. Tokens without intent leave any AI agent filling silence with generic guesses.

## What you get

A DESIGN.md file at the right location, with:

- **YAML frontmatter** holding all design tokens (colors, typography, rounded, spacing, components).
- **Markdown prose** explaining intent, semantic meaning, and where tokens must never appear.
- **At least 3 entries in "Do's and Don'ts"** (the highest-leverage section).
- **A Motion section** with `prefers-reduced-motion` handling (accessibility default).
- **0 errors from the linter** (validated before save).

Plus a one-line snippet to add to your agent instructions file (CLAUDE.md, AGENTS.md, Cursor rules, or whatever your tool uses), so the agent actually reads DESIGN.md when working on UI:

```
All UI must follow DESIGN.md at the repo root.
```

Without that line, most agents won't auto-load DESIGN.md. The skill reminds you at the end and suggests the right file based on context.

## Files in this folder

| File | Audience | Purpose |
|------|----------|---------|
| `SKILL.md` | The AI agent | Full interactive flow, instructions, persona, dos and donts |
| `template.md` | The AI agent | Canonical skeleton with placeholders for all 8 sections |
| `reference.md` | The AI agent | Naming conventions, linter rules, prose patterns, real example briefs |
| `README.md` | Humans | This file: quick reference for whoever invokes or shares the skill |

## Key design choices

A few decisions in the skill worth knowing about, in case you want to tweak them later:

1. **Two color depths.** "Lean" (4 colors: primary, secondary, tertiary, neutral) for content sites and fast iteration. "Material 3" (full role system, ~50 tokens) for production apps. The published Google Labs examples all use Material 3.

2. **Motion is not in the official spec, but the skill includes it.** Unknown sections are preserved by the linter, so adding `## Motion` is safe. Motion is the most under-specified part of most design systems, so the skill includes it by default with `prefers-reduced-motion` honored.

3. **Quality gate uses the official CLI**, not a research agent. `npx @google/design.md lint` is the canonical validator. It catches broken token references, contrast-ratio failures, section-order issues, and orphaned tokens.

4. **State variants are separate entries** (`button-primary-hover`), not nested objects. This is the official spec pattern.

## Naming gotcha

This skill produces `DESIGN.md` (uppercase), the visual design system file.

There's also a `design.md` (lowercase) convention from spec-driven development (Kiro, GitHub Spec Kit) that's used for technical architecture specs. Different file, different purpose. If you ask for "design.md" lowercase, the skill will check which you mean.

## When to use this skill

**Good fits:**
- Starting a new project and want to lock in a distinctive aesthetic before coding.
- Studying competitor or inspiration sites to capture what makes them feel good.
- Existing codebase has implicit design choices buried in Tailwind config; want them surfaced and intentional.
- Want to merge multiple inspirations into one cohesive system before building.

**Not a fit:**
- Quick mockups or one-off prototypes (overkill).
- Pure architecture documentation (use `design.md` lowercase or PRD instead).
- Design systems that need full Figma component libraries (use the figma-generate-library skill alongside this).

## Reference: the format itself

If you want to skip the skill and write a DESIGN.md by hand, the official spec lives at https://github.com/google-labs-code/design.md. The minimum viable file needs: `name`, `colors.primary`, one typography token, and one component. The linter accepts that.

The CLI tools worth knowing:

```bash
npx @google/design.md lint DESIGN.md                                  # validate
npx @google/design.md export --format css-tailwind DESIGN.md          # to Tailwind v4 CSS
npx @google/design.md export --format json-tailwind DESIGN.md         # to Tailwind v3 config
npx @google/design.md export --format dtcg DESIGN.md                  # to W3C tokens
npx @google/design.md diff DESIGN-v1.md DESIGN-v2.md                  # compare versions
```

## Version

Built 2026-05-28. The DESIGN.md format itself is at version `alpha`; expect schema changes as the spec evolves.

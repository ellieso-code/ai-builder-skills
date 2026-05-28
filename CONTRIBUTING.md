# Contributing

## Skill format

Every skill lives in its own folder under `skills/` and must contain a `SKILL.md` file with this frontmatter:

```markdown
---
name: skill-name
description: One sentence on when Claude should use this skill. Be specific about the trigger.
---

# Skill body
...
```

The `description` is what Claude reads to decide whether to invoke the skill, so it should describe the trigger clearly (e.g., "Use when the user asks to create a PRD or feature spec").

Supporting files (templates, reference docs, scripts) can live alongside `SKILL.md` in the same folder.

## Adding a skill

1. Create a folder under `skills/<your-skill-name>/`
2. Add `SKILL.md` with proper frontmatter
3. Test it locally by copying to `~/.claude/skills/` and invoking it
4. Open a PR with a short description of what the skill does and who it's for

## Style

- No personal data in skill files (API keys, contact info, internal company details)
- Skills should be reusable by people outside the original author's context
- Prefer clear, plain language over jargon

# DESIGN.md Reference

Naming conventions, linter rules, example prose patterns, and source pointers. Read this when you need spec-level detail not covered in `SKILL.md`.

## Canonical Section Order

| # | Section | Aliases | Status |
|---|---------|---------|--------|
| 1 | Overview | "Brand & Style" | SHOULD have |
| 2 | Colors | none | SHOULD have |
| 3 | Typography | none | SHOULD have |
| 4 | Layout | "Layout & Spacing" | NICE TO HAVE |
| 5 | Elevation & Depth | "Elevation" | NICE TO HAVE |
| 6 | Shapes | none | NICE TO HAVE |
| 7 | Components | none | SHOULD have |
| 8 | Do's and Don'ts | none | NICE TO HAVE |

**Hard rules:**
- Sections present MUST appear in this order.
- Duplicate `##` headings cause the linter to REJECT the file (hard error).
- Unknown sections (`## Motion`, `## Inspirations`) are preserved without erroring.

## YAML Schema

```yaml
version: <string>           # optional, current accepted: "alpha"
name: <string>              # required for any usable file
description: <string>       # optional
colors:
  <token-name>: <Color>
typography:
  <token-name>: <Typography>
rounded:
  <scale-level>: <Dimension>
spacing:
  <scale-level>: <Dimension | number>
components:
  <component-name>:
    <property>: <string | token reference>
```

## Type Definitions

| Type | Format | Examples |
|------|--------|----------|
| `Color` | `#` + hex (sRGB), quoted string | `"#1A1C1E"` |
| `Dimension` | number + unit (`px`, `em`, `rem`) | `48px`, `1.6rem`, `-0.02em` |
| `Token Reference` | `{path.to.token}` quoted | `"{colors.primary}"` |
| Unitless number | bare number (for ratios) | `1.6`, `1.1` |

## Typography Object Schema

```yaml
typography:
  <name>:
    fontFamily: <string>           # e.g. "Public Sans"
    fontSize: <Dimension>          # e.g. 48px, 3rem
    fontWeight: <number|string>    # e.g. 600 or "600"
    lineHeight: <Dimension|number> # 24px OR 1.5 (multiplier, recommended)
    letterSpacing: <Dimension>     # e.g. -0.02em, 0.1em
    fontFeature: <string>          # optional, CSS font-feature-settings
    fontVariation: <string>        # optional, CSS font-variation-settings
```

## Component Property Whitelist

Per spec, exactly these properties. Unknown properties trigger a warning but are accepted:

- `backgroundColor` (Color or token ref)
- `textColor` (Color or token ref)
- `typography` (Typography token ref, can reference composite)
- `rounded` (Dimension or token ref)
- `padding` (Dimension)
- `size` (Dimension)
- `height` (Dimension)
- `width` (Dimension)

## Token Reference Rules

- Wrap in curly braces, dot-pathed: `{colors.primary-60}`
- Most groups require references to **primitive values**, not groups.
- Inside the `components` section only, you may reference **composite** values like `{typography.label-md}`.

## Naming Conventions (recommended)

- **Colors:** `primary`, `secondary`, `tertiary`, `neutral`, `surface`, `on-surface`, `error`
- **Typography:** `display`, `headline-lg`, `headline-md`, `body-lg`, `body-md`, `body-sm`, `label-md`, `label-sm`
- **Rounded:** `none`, `sm`, `md`, `lg`, `xl`, `full`
- **Spacing:** `xs`, `sm`, `md`, `lg`, `xl`, `base`, `gutter`, `margin`
- **Components:** kebab-case with type prefix: `button-primary`, `card-profile`, `input-field`, `badge-status`
- **Variants:** append state to base name: `button-primary-hover`, `button-primary-active`, `list-item-hover`

## Linter Rules (Official CLI)

The official linter (`npx @google/design.md lint <file>`) runs 8 rules:

| Rule | Severity | What it catches |
|------|----------|-----------------|
| `broken-ref` | error | `{colors.foo}` references a token that doesn't exist |
| `missing-primary` | warning | Colors defined but no `primary` color |
| `contrast-ratio` | warning | Component bg/text below WCAG AA 4.5:1 |
| `orphaned-tokens` | warning | Color tokens never used by any component |
| `token-summary` | info | Count of tokens per section |
| `missing-sections` | info | Optional sections (spacing, rounded) absent |
| `missing-typography` | warning | Colors defined but no typography |
| `section-order` | warning | Sections out of canonical order |

Exit code `1` if errors found, `0` otherwise.

## CLI Commands

```bash
# Validate structure
npx @google/design.md lint DESIGN.md

# Compare two versions for regressions
npx @google/design.md diff DESIGN.md DESIGN-v2.md

# Export to Tailwind v3 config
npx @google/design.md export --format json-tailwind DESIGN.md > tailwind.theme.json

# Export to Tailwind v4 CSS
npx @google/design.md export --format css-tailwind DESIGN.md > theme.css

# Export to W3C Design Tokens Format
npx @google/design.md export --format dtcg DESIGN.md > tokens.json

# Output the spec itself (useful for prompt context)
npx @google/design.md spec
```

## Example Overview Briefs (real, from Google Labs examples)

**Paws & Paths** (pet walking app):
> The design system is built to evoke the joyful energy of a walk in the park balanced with the reliability of a premium professional service. The brand personality is optimistic, trustworthy, and active.
>
> The chosen style is **Modern Corporate** with a friendly, human-centric twist. It utilizes clean layouts and significant whitespace to reduce cognitive load for busy pet owners.

**Atmospheric Glass** (weather app):
> This design system centers on a high-fidelity Glassmorphism aesthetic designed to evoke a sense of clarity, depth, and modern sophistication. The brand personality is ethereal yet functional.
>
> The UI relies on a "vibrant-minimalist" approach: the background provides the energy through multi-colored abstract gradients, while the interface elements act as frosted crystalline lenses.

**Totality Festival** (event/cultural):
> The design system captures the visceral tension and awe of a solar eclipse. The aesthetic is "Cosmic Premium," blending the stark mystery of deep space with the explosive brilliance of the solar corona.
>
> The design system utilizes **Glassmorphism** and **High-Contrast** movements. Surfaces appear as translucent obsidian slabs floating over nebula-like gradients.

**Heritage** (editorial/news):
> Architectural Minimalism meets Journalistic Gravitas. The UI evokes a premium matte finish: a high-end broadsheet or contemporary gallery.

## Pattern Analysis: What Makes Overviews Work

- They name a **specific style movement** (Glassmorphism, Modern Corporate, etc.) so the agent has anchor references.
- They include a **target audience** in passing.
- They use **one concrete metaphor** (frosted crystalline lenses, broadsheet, obsidian slabs).
- They explicitly **say what the UI is NOT**.
- Length: **80 to 150 words**, two paragraphs.

## Three-Layer Prose Pattern (for every token)

Every token entry in prose should carry three layers:

1. **Value** (the hex/dimension, already in YAML)
2. **Rationale** (why this exists, what it represents)
3. **Restriction** (where it must not be used, max usage per screen)

Example:
> Primary (Boston Clay, #B8422E): The sole driver for interaction. Used for CTAs and active states only. Never used as a background, never decorative. One per screen.

## 15 Seed Rules for Do's and Don'ts

**Color**
1. Do use the primary color only for the single most important action per screen.
2. Do reserve the tertiary/accent color exclusively for primary CTAs and critical highlights.
3. Don't use the accent color as a background or for decorative elements.
4. Do use neutral colors for all primary text and structural surfaces.

**Typography**
5. Don't use more than two font weights on a single screen.
6. Don't pair more than two type families in the same view.
7. Do reserve uppercase labels with generous letter spacing for technical data only.

**Layout**
8. Do follow a strict 8px spacing scale; use 4px half-step only for micro-adjustments.
9. Don't crowd elements; use `lg` or `xl` spacing for vertical section separation.
10. Do center content with a maximum width on larger screens to maintain focus.

**Shape / elevation**
11. Don't mix rounded and sharp corners in the same view.
12. Don't use heavy drop shadows; prefer tonal layers or subtle ambient shadows.
13. Do maintain consistent border treatments on glass surfaces.

**Accessibility**
14. Do maintain WCAG AA contrast ratios (4.5:1 for normal text, 3:1 for large text).
15. Don't rely on color alone to convey state; pair with icons or labels.

## Companion Files (Standard Output Set)

In every official example, `DESIGN.md` ships with three siblings. Consider offering to generate these:

- `README.md` (describes the example, intended UI)
- `design_tokens.json` (DTCG export of tokens)
- `tailwind.config.js` (Tailwind export of tokens)

Generate via:
```bash
npx @google/design.md export --format dtcg DESIGN.md > design_tokens.json
npx @google/design.md export --format json-tailwind DESIGN.md > tailwind.theme.json
```

## Source URLs

For pulling fresh content from the canonical repo:

- Spec: `https://raw.githubusercontent.com/google-labs-code/design.md/main/docs/spec.md`
- README: `https://raw.githubusercontent.com/google-labs-code/design.md/main/README.md`
- Paws & Paths example: `https://raw.githubusercontent.com/google-labs-code/design.md/main/examples/paws-and-paths/DESIGN.md`
- Atmospheric Glass example: `https://raw.githubusercontent.com/google-labs-code/design.md/main/examples/atmospheric-glass/DESIGN.md`
- Totality Festival example: `https://raw.githubusercontent.com/google-labs-code/design.md/main/examples/totality-festival/DESIGN.md`

## Status

The DESIGN.md format is at version `alpha`. Spec, schema, and CLI are under active development. Expect changes.

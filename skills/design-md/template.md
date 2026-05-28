# DESIGN.md Canonical Template

The full skeleton with placeholders. All 8 canonical sections in spec order, plus optional Motion and Inspirations extensions. Replace `<ANGLE_BRACKETS>` with real content.

````markdown
---
version: alpha
name: <SYSTEM_NAME>
description: <ONE_LINE_DESCRIPTION_OPTIONAL>

colors:
  # Lean palette (4 colors). Switch to Material 3 full system if production-grade.
  primary: "<HEX>"
  secondary: "<HEX>"
  tertiary: "<HEX>"
  neutral: "<HEX>"
  surface: "<HEX>"
  on-surface: "<HEX>"
  error: "<HEX>"

  # Material 3 extension (uncomment if using full role system)
  # on-primary: "<HEX>"
  # primary-container: "<HEX>"
  # on-primary-container: "<HEX>"
  # surface-container: "<HEX>"
  # surface-container-high: "<HEX>"
  # surface-container-highest: "<HEX>"
  # on-surface-variant: "<HEX>"
  # outline: "<HEX>"
  # outline-variant: "<HEX>"

typography:
  display:
    fontFamily: <FONT_NAME>
    fontSize: <SIZE>px
    fontWeight: "<WEIGHT>"
    lineHeight: <LH>
    letterSpacing: <LS>em
  headline-lg:
    fontFamily: <FONT_NAME>
    fontSize: <SIZE>px
    fontWeight: "<WEIGHT>"
    lineHeight: <LH>
  headline-md:
    fontFamily: <FONT_NAME>
    fontSize: <SIZE>px
    fontWeight: "<WEIGHT>"
    lineHeight: <LH>
  body-lg:
    fontFamily: <FONT_NAME>
    fontSize: 18px
    fontWeight: "400"
    lineHeight: 1.6
  body-md:
    fontFamily: <FONT_NAME>
    fontSize: 16px
    fontWeight: "400"
    lineHeight: 1.5
  body-sm:
    fontFamily: <FONT_NAME>
    fontSize: 14px
    fontWeight: "400"
    lineHeight: 1.5
  label-md:
    fontFamily: <FONT_NAME>
    fontSize: 14px
    fontWeight: "500"
    lineHeight: 1.4
    letterSpacing: 0.05em
  label-sm:
    fontFamily: <FONT_NAME>
    fontSize: 12px
    fontWeight: "500"
    lineHeight: 1.3
    letterSpacing: 0.08em

rounded:
  none: 0px
  sm: 4px
  md: 8px
  lg: 12px
  xl: 16px
  full: 9999px

spacing:
  base: 8px
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 40px
  gutter: 24px
  margin: 32px

components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.label-md}"
    rounded: "{rounded.lg}"
    padding: "{spacing.md}"
    height: 48px
  button-primary-hover:
    backgroundColor: "{colors.primary-container}"

  button-secondary:
    backgroundColor: transparent
    textColor: "{colors.primary}"
    typography: "{typography.label-md}"
    rounded: "{rounded.lg}"
    padding: "{spacing.md}"
    height: 48px
  button-secondary-hover:
    backgroundColor: "{colors.surface-container}"

  card-default:
    backgroundColor: "{colors.surface}"
    rounded: "{rounded.xl}"
    padding: "{spacing.lg}"

  input-field:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.on-surface}"
    typography: "{typography.body-md}"
    rounded: "{rounded.md}"
    padding: "{spacing.sm}"
    height: 48px

  list-item:
    backgroundColor: transparent
    rounded: "{rounded.md}"
    padding: "{spacing.sm}"
  list-item-hover:
    backgroundColor: "{colors.surface-container}"

  badge:
    backgroundColor: "{colors.tertiary}"
    textColor: "{colors.on-tertiary}"
    typography: "{typography.label-sm}"
    rounded: "{rounded.full}"
    padding: "{spacing.xs}"

# Optional: Motion (non-canonical, but linter-tolerated)
# motion:
#   duration-fast: 150ms
#   duration-base: 250ms
#   duration-slow: 400ms
#   ease-standard: cubic-bezier(0.4, 0, 0.2, 1)
#   ease-decel: cubic-bezier(0, 0, 0.2, 1)
#   ease-accel: cubic-bezier(0.4, 0, 1, 1)
---

# <SYSTEM_NAME>

## Overview

<TWO_TO_THREE_SENTENCE_BRAND_BRIEF: emotional response, brand personality adjectives, target audience.>

The chosen style is **<NAMED_MOVEMENT>** with <variant_or_twist>. <ONE_CONCRETE_METAPHOR>. <WHAT_IT_MUST_NOT_FEEL_LIKE>.

## Colors

<ONE_SENTENCE_PALETTE_PHILOSOPHY: what role color plays in the system.>

- **Primary (<DESCRIPTIVE_NAME>, <HEX>):** <usage rule, when to use, when to never use>.
- **Secondary (<DESCRIPTIVE_NAME>, <HEX>):** <usage rule>.
- **Tertiary (<DESCRIPTIVE_NAME>, <HEX>):** <usage rule, typically the accent>.
- **Neutral (<DESCRIPTIVE_NAME>, <HEX>):** <foundation surfaces and text>.

## Typography

<ONE_SENTENCE_TYPE_STRATEGY: font choice rationale.>

- **<FONT_FAMILY>** is the voice of the brand. <Why this font; what it suggests>.
- **Headlines** use <weight> with <letter-spacing rule> for <emotional intent>.
- **Body** uses <weight> with line-height <number> for <readability intent>.
- **Labels** use <case treatment, letter spacing, where they appear>.

## Layout

<ONE_SENTENCE_LAYOUT_PHILOSOPHY: grid model, whitespace stance.>

A <NUMBER>-column grid is used for desktop layouts, with <outer-margin description>. Spacing follows an <8px or 4px> base unit. <Negative space stance: generous vs dense>. <Container max-width and centering rules>.

## Elevation & Depth

<ONE_SENTENCE_DEPTH_PHILOSOPHY: shadows vs tonal layers vs glass.>

- **Level 1 (Base):** <description>.
- **Level 2 (Cards/Panels):** <description, shadow specs if any>.
- **Level 3 (Modals/Elevated):** <description>.
- **Interaction feedback:** <how depth changes on hover/focus>.

## Shapes

<ONE_SENTENCE_SHAPE_LANGUAGE: rounded vs sharp, why.>

- **Buttons:** `rounded-lg` (12px) for a <emotional intent>.
- **Cards:** `rounded-xl` (16px) for <emotional intent>.
- **Inputs:** `rounded-md` (8px) for <structural alignment>.
- **Icons:** <stroke/cap style>.

## Components

### Buttons

<PROSE_ABOUT_BUTTON_INTENT_AND_STATES: when primary vs secondary, hover behavior, disabled state.>

### Cards & Surfaces

<PROSE_ABOUT_CARDS: when to use, elevation behavior, content density.>

### Inputs & Forms

<PROSE_ABOUT_INPUT_BEHAVIOR: focus states, error states, labels and helper text.>

### Lists & Navigation

<PROSE_ABOUT_LIST_BEHAVIOR: hover, selected state, touch targets.>

## Motion

<ONE_SENTENCE_MOTION_PHILOSOPHY: functional vs decorative.>

- **Duration:** 150ms for state changes, 250ms for layout shifts, 400ms for entrance/exit.
- **Easing:** `ease-in-out` for state changes, `ease-out` for entrances, `ease-in` for exits.
- **Hover:** Components shift backgroundColor over 150ms; cards lift 2px on the Y axis.
- **Page transitions:** Stagger child reveals at 50ms intervals.
- **Reduced motion:** Always respect `prefers-reduced-motion`. Replace transforms with opacity fades when set.

## Do's and Don'ts

- Do <RULE>.
- Do <RULE>.
- Do maintain WCAG AA contrast ratios (4.5:1 for normal text).
- Don't <RULE>.
- Don't <RULE>.
- Don't use more than two font weights on a single screen.
- Don't rely on color alone to convey state.
````

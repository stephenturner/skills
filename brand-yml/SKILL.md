---
name: brand-yml
description: Create a _brand.yml file for Quarto projects that captures brand colors, typography, and identity in a structured YAML format. Use this skill whenever the user mentions _brand.yml, brand.yml, Quarto branding, brand identity for Quarto, or wants consistent colors and fonts across Quarto documents, websites, presentations, or Shiny apps. Also trigger when the user asks to extract brand colors or fonts from a website, PDF brand guidelines, or style guide for use with Quarto. Even if the user just says "set up my brand" or "make my Quarto docs match our brand," this is the right skill.
---

# brand-yml: Create Brand Identity Files for Quarto

Generate a valid `_brand.yml` file from brand guidelines, a website, a description, or an existing file that needs refinement.

## Before You Start

Read the full specification:

```
Read references/brand-yml-spec.md
```

This reference contains every allowed field, syntax variants, and the complete annotated example. Consult it whenever you are unsure about field names, nesting, or allowed values.

## Workflow

### 1. Gather Brand Information

Determine the input source. The user may provide one or more of these:

**Website URL** -- Use `web_fetch` to retrieve the page. Extract CSS custom properties, meta theme colors, prominent hex values, font-family declarations, and Open Graph metadata. If the page links to a CSS file, fetch that too. Look for Google Fonts or Bunny Fonts `<link>` tags.

**Uploaded brand guidelines (PDF, image, or document)** -- Read the uploaded file. Extract named colors (with hex values), font families, usage rules (which font is for headings vs. body), and logo file references.

**Manual description** -- The user describes their colors, fonts, and identity in plain text. Ask clarifying questions only if critical information is missing (primary color and at least one font are the minimum viable brand).

**Existing `_brand.yml`** -- The user uploads or pastes a file to fix, extend, or modernize. Validate it against the spec, identify issues, and propose corrections.

If the source is ambiguous, ask which approach the user prefers.

### 2. Research and Resolve

Use web search to fill gaps when helpful:

- Match proprietary or unknown fonts to the closest Google Fonts equivalent. Always note the substitution and the original font name in a YAML comment.
- Verify exact hex values for well-known brands if the user gives a name but not a code (e.g., "use UVA colors").
- Check Google Fonts for available weights and styles when setting up `typography.fonts` entries.

### 3. Assemble the `_brand.yml`

Follow these rules strictly:

**Structure rules:**
- Include only fields that apply. An empty or default-value field should be omitted entirely.
- Use the simple string form whenever the complex mapping form is not needed. For example, `name: Acme` instead of `name: { short: Acme }`.
- Keep `color.palette` flat. No nesting. Names follow Sass variable conventions (lowercase, hyphens).
- Prefer hex color values everywhere. Use 6-digit hex (`#RRGGBB`).
- Refer to `color.palette` names in theme color fields (`primary`, `success`, etc.) whenever a palette entry exists for that color.
- Never set `color` inside `typography.base`. Base text color comes from `color.foreground` automatically.

**Typography rules:**
- For `source: google` or `source: bunny`, list only the weights and styles actually used by the brand. Omit `weight` and `style` if all defaults are fine.
- Prefer `source: google` over `source: bunny` for web fonts.
- For `source: file`, include placeholder paths and add a comment explaining where to download the font files. Every `source: file` font must have at least one entry in `files`.
- For proprietary fonts, suggest a Google Fonts substitute, include the substitute as a `source: google` entry, and add a comment noting the original proprietary font name.

**Logo rules:**
- Use placeholder file paths under a `logos/` directory.
- Add a comment telling the user to download and place the logo files next to `_brand.yml`.

**Color palette guidance:**
- When a brand provides a range of shades/tints, pick the midpoint as the palette entry.
- Create aliases for Bootstrap primary color names (blue, red, green, orange, etc.) when a brand color maps naturally to one.
- Map semantic theme colors (`primary`, `success`, `warning`, `danger`, `info`) to palette names when possible.

**`defaults` section:**
- Only include if the user specifically asks for Bootstrap, Quarto format, or Shiny theme customizations. Do not add it by default.

### 4. Validate

Before presenting the output, mentally check:

- Every `color.palette` reference used in theme colors actually exists in the palette.
- No circular references in color aliases.
- Font families referenced in `base`, `headings`, `monospace` have a matching entry in `typography.fonts`.
- No `color` key inside `typography.base`.
- All `source: file` fonts have a `files` list with at least one entry.
- The YAML is syntactically valid (proper indentation, quoting of hex values, no trailing commas).
- Hex color values are quoted (YAML interprets unquoted `#` as a comment).

### 5. Deliver the Output

**Short/simple brands** (fewer than ~40 lines of YAML): Present the `_brand.yml` in a fenced YAML code block in chat.

**Complex brands or when the user requests a file**: Create the file at `/home/claude/_brand.yml`, then copy it to `/mnt/user-data/outputs/_brand.yml` and present it with the `present_files` tool.

**Always** include a brief summary after the YAML listing:
- The colors extracted and their roles.
- The fonts chosen (noting any substitutions).
- Any fields the user may want to customize further.
- Instructions for downloading logo or font files if placeholders were used.

## Common Patterns

### University or Organization with Known Colors

Search for the official hex values. Universities often publish brand guides online. Use those exact values rather than guessing from a screenshot.

### Website Extraction

After fetching the page and CSS:
1. Look for CSS custom properties (`--brand-primary`, `--color-accent`, etc.) first.
2. Fall back to computed styles on key elements (header background, link color, heading font-family).
3. Check `<link>` tags for Google Fonts imports to identify font families and weights.

### Minimal Brand

If the user only provides a primary color and a font name, generate a minimal file with just `color.primary`, `color.foreground`, `color.background`, and `typography.base`. Do not pad with unnecessary fields.

## What Not to Do

- Do not invent colors or fonts the user did not provide or that you cannot verify.
- Do not include the `defaults` section unless explicitly requested.
- Do not use `color` inside `typography.base`.
- Do not include empty sections or fields with default values that add no information.
- Do not use RGB, HSL, or named CSS colors. Always convert to hex.

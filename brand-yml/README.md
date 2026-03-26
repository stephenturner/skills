# Claude skill: _brand.yml

A [Claude skill](https://code.claude.com/docs/en/skills) that generates [`_brand.yml`](https://posit-dev.github.io/brand-yml/) files for Quarto projects. Give it a website URL, a PDF of brand guidelines, a plain-text description of your brand, or an existing `_brand.yml` to refine, and it will produce a valid, well-structured brand identity file ready to use with Quarto documents, websites, presentations, and Shiny apps.

The skill's specification reference is adapted from the [brand-yml LLM prompt](https://posit-dev.github.io/brand-yml/articles/llm-brand-yml-prompt/).

See [my recent blog post here](https://doi.org/10.59350/f9dyf-zwc64) for more details.

## Installation

**Option 1: Download ZIP**

Click the green **Code** button at the top of this repo, then **Download ZIP**. Extract the ZIP and add the folder to your Claude skills directory.

**Option 2: Releases**

Go to the [Releases](https://github.com/stephenturner/skill-brand-yml/releases) page and download the latest `.skill` file. Add it to your Claude skills in [customize/skills](https://claude.ai/customize/skills) on the web, or double-click it if you have Claude Desktop installed.

**Option 3: Build it yourself**

Build a `.skill` file from the source code, then add it to your Claude skills as described above.

```sh
git clone https://github.com/stephenturner/skill-brand-yml.git
cd skill-brand-yml
zip -r brand-yml.skill SKILL.md references/
```

## What it does

When triggered, the skill will:

1. **Gather brand information** from whatever source you provide (URL, uploaded file, description, or existing YAML).
2. **Research and resolve** unknowns using web search, such as looking up official hex values for a university or finding the closest Google Fonts match for a proprietary typeface.
3. **Assemble a valid `_brand.yml`** following the [brand-yml specification](https://posit-dev.github.io/brand-yml/), including color palettes, semantic theme colors, typography definitions, logo placeholders, and metadata.
4. **Validate** the output against the spec before presenting it.
5. **Deliver** the result as a YAML code block in chat or as a downloadable file, depending on complexity.

## Examples

**Extract brand identity from a website:**

> Create a `_brand.yml` for my Quarto project based on https://posit.co

**Use well-known institutional colors:**

> Create a `_brand.yml` using the UVA School of Data Science brand. Use their official colors and find Google Fonts that are close to their brand fonts.

**Build from a manual description:**

> I need a `_brand.yml` with these colors: navy `#1B2A4A`, coral `#FF6B6B`, cream `#FFF8F0`. Use Inter for body text and Playfair Display for headings.

**Refine an existing file:**

> Here's my current `_brand.yml`. Can you check it against the spec and fix any issues?

## Skill contents

```
brand-yml-skill/
├── README.md
├── SKILL.md
└── references/
    └── brand-yml-spec.md
```

## Links

- [brand.yml documentation](https://posit-dev.github.io/brand-yml/)
- [brand.yml LLM prompt reference](https://posit-dev.github.io/brand-yml/articles/llm-brand-yml-prompt/)
- [Claude custom skills documentation](https://code.claude.com/docs/en/skills)

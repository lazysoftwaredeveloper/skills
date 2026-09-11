# Skills

Reusable Agent Skills by Lazy Software Developer.

## Available skills

| Skill | Purpose |
|---|---|
| `keyword-opportunity-research` | Evidence-driven keyword, SERP, user-pain and SaaS opportunity research |
| `research-to-facts` | Convert raw research into atomic, source-independent facts while preserving provenance |
| `fact-quality-control` | Deduplicate facts, resolve/flag conflicts, score confidence and set publication policy |
| `publication-context-builder` | Build writer-ready context while isolating internal sources, URLs and research notes |
| `publication-qa` | Final publication gate for source leakage, meta-language and unsupported claims |

## Content-engineering pipeline

The four content-engineering skills are designed to work independently or as a pipeline:

```text
Raw Research
    ↓
research-to-facts
    ↓
fact-quality-control
    ↓
Canonical Knowledge
    ↓
publication-context-builder
    ↓
Writer / LLM
    ↓
publication-qa
    ↓
Publish
```

The key design principle is that provenance stays available for audit and verification, but is not automatically exposed to the writer or the published page.

## Installed grouping

When installed with `npx skills add`, the repository plugin name is `lazysoftwaredeveloper-skills`. Installed skills from this repository can therefore appear together under that plugin in:

```bash
npx skills list -g
```

Repository layout is flat under `skills/`:

```text
skills/
├── keyword-opportunity-research/
├── research-to-facts/
├── fact-quality-control/
├── publication-context-builder/
└── publication-qa/
```

There is no additional category directory inside `skills/`.

## Install

List skills in this repository:

```bash
npx skills add lazysoftwaredeveloper/skills --list
```

Install one skill:

```bash
npx skills add lazysoftwaredeveloper/skills --skill research-to-facts
```

Install all skills from this repository:

```bash
npx skills add lazysoftwaredeveloper/skills --all
```

# Skills

Reusable Agent Skills by Lazy Software Developer.

## Available skills

| Skill | Purpose |
|---|---|
| `keyword-opportunity-research` | Evidence-driven keyword, SERP, user-pain and SaaS opportunity research |
| `research-to-facts` | Convert raw research into atomic, source-independent facts while preserving provenance |
| `fact-quality-control` | Deduplicate facts, resolve/flag conflicts, score confidence and set publication policy |
| `publication-context-builder` | Build writer-ready context while isolating internal sources, URLs and research notes |
| `expert-content-writer` | Turn clean publication context into reader-facing expert content using domain profiles |
| `publication-qa` | Final publication gate for source leakage, meta-language and unsupported claims |

## Content-engineering pipeline

The five content-engineering skills are designed to work independently or as a pipeline:

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
expert-content-writer
    ↓
publication-qa
    ↓
Publish
```

The key design principle is separation of concerns:

- provenance remains available upstream for audit and verification;
- the context builder decides what the writer is allowed to see;
- the writer controls expression, not truth;
- publication QA challenges the finished draft before release.

### Writer profiles

`expert-content-writer` currently includes:

- `game-guide`
- `software-tutorial`
- `product-review`

Profiles change structure and editorial style without changing the underlying facts.

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
├── expert-content-writer/
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
npx skills add lazysoftwaredeveloper/skills --skill expert-content-writer
```

Install all skills from this repository:

```bash
npx skills add lazysoftwaredeveloper/skills --all
```

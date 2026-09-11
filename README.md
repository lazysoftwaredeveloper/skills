# Skills

Reusable Agent Skills by Lazy Software Developer.

## Available skills

| Skill | Purpose |
|---|---|
| `keyword-opportunity-research` | Evidence-driven keyword, SERP, user-pain and SaaS opportunity research |
| `research-to-facts` | Convert raw research into atomic, source-independent facts while preserving provenance |
| `fact-quality-control` | Deduplicate facts, resolve/flag conflicts, score confidence and set publication policy |
| `content-type-router` | Route a Page Brief to the correct writer profile, publication mode and content modules |
| `publication-context-builder` | Build writer-ready context while isolating internal sources, URLs and research notes |
| `expert-content-writer` | Turn clean publication context into reader-facing expert content using domain profiles |
| `publication-qa` | Final publication gate for source leakage, meta-language and unsupported claims |

## Content-engineering pipeline

The six content-engineering skills are designed to work independently or as a pipeline:

```text
Raw Research
    ↓
research-to-facts
    ↓
fact-quality-control
    ↓
Canonical Knowledge
      +
Page Brief
    ↓
content-type-router
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
- the router decides what kind of page is being produced;
- the context builder decides what the writer is allowed to see;
- the writer controls expression, not truth;
- publication QA challenges the finished draft before release.

## Complete game-site writing system

`expert-content-writer` includes four complementary game profiles:

| Page job | Profile |
|---|---|
| Game introduction, How to Play, beginner mental model | `game-overview` |
| Mechanics, controls, items, rules, FAQ | `game-reference` |
| Boss, quest, unlock, achievement, collectible, strategy | `game-guide` |
| Level, stage, chapter or puzzle with exact ordered actions | `game-walkthrough` |

A complete level-based game site can therefore use one shared Knowledge Store while choosing different presentation rules per page:

```text
Homepage / How to Play       → game-overview
Controls / Mechanics / FAQ   → game-reference
Boss / Quest / Unlock        → game-guide
Level 1..N / Puzzle          → game-walkthrough
```

For walkthroughs, sequence and state are treated as semantic facts. Internal provenance can be removed, but `sequence / starting_state / direction / count / outcome` must survive when required for correctness.

Other built-in writer profiles:

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
├── content-type-router/
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
npx skills add lazysoftwaredeveloper/skills --skill content-type-router
```

Install all skills from this repository:

```bash
npx skills add lazysoftwaredeveloper/skills --all
```

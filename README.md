# Skills

Reusable Agent Skills by Lazy Software Developer.

## Installed grouping

When installed with `npx skills add`, the repository plugin name is `lazysoftwaredeveloper-skills`. Installed skills from this repository can therefore appear together under that plugin in:

```bash
npx skills list -g
```

Repository layout:

```text
skills/
└── keyword-opportunity-research/
    ├── SKILL.md
    ├── README.md
    └── references/
```

There is no additional category directory inside `skills/`.

## Install

List skills in this repository:

```bash
npx skills add lazysoftwaredeveloper/skills --list
```

Install `keyword-opportunity-research`:

```bash
npx skills add lazysoftwaredeveloper/skills --skill keyword-opportunity-research
```

Install all skills from this repository:

```bash
npx skills add lazysoftwaredeveloper/skills --all
```

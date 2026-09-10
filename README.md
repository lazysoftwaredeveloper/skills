# Skills

Reusable Agent Skills organized by category.

## Groups

### Research

Skills for keyword research, website analysis, SERP exploration, demand discovery, and SaaS opportunity research.

- `keyword-opportunity-research`

Repository layout:

```text
skills/
└── research/
    └── keyword-opportunity-research/
        ├── SKILL.md
        ├── README.md
        └── references/
```

## Install

List all skills in this repository:

```bash
npx skills add lazysoftwaredeveloper/skills --list
```

Install the whole `research` group by targeting its repository subtree:

```bash
npx skills add https://github.com/lazysoftwaredeveloper/skills/tree/main/skills/research
```

Install one skill:

```bash
npx skills add lazysoftwaredeveloper/skills --skill keyword-opportunity-research
```

Install all skills from the repository:

```bash
npx skills add lazysoftwaredeveloper/skills --all
```

## Group metadata

`skills.sh.json` declares the same `Research` grouping for catalog/discovery surfaces that support the skills.sh grouping schema.

Note: the current `skills` CLI does not yet expose a native `--group` flag. The subtree install command above is the repository-level way to install a category as a unit.

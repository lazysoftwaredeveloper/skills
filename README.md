# Skills

Reusable Agent Skills by Lazy Software Developer.

## Repository layout

```text
.claude-plugin/
├── plugin.json
└── marketplace.json
skills/
└── research/
    └── keyword-opportunity-research/
        ├── SKILL.md
        ├── README.md
        └── references/
```

## Install

List skills in this repository:

```bash
npx skills add lazysoftwaredeveloper/skills --list
```

Install one skill globally:

```bash
npx skills add lazysoftwaredeveloper/skills --skill keyword-opportunity-research -g
```

Install all skills globally:

```bash
npx skills add lazysoftwaredeveloper/skills -g
```

## Grouping in `npx skills list -g`

The grouping shown by `npx skills list -g` is plugin-based, not directory/category-based.

This repository declares a Claude plugin manifest at `.claude-plugin/plugin.json` with:

```json
{
  "name": "lazysoftwaredeveloper-skills",
  "skills": [
    "./skills/research/keyword-opportunity-research"
  ]
}
```

The `skills` CLI persists that plugin name when installing skills. As a result, globally installed skills from this manifest should be shown under a heading derived from `lazysoftwaredeveloper-skills`, analogous to `Mattpocock Skills` for `mattpocock-skills`.

Expected shape:

```text
Lazysoftwaredeveloper Skills
  keyword-opportunity-research ~/.agents/skills/keyword-opportunity-research
    Agents: ...
```

When adding more skills to this repository, add their paths to the `skills` array in `.claude-plugin/plugin.json` so they are associated with the same plugin group.

If the skill was installed before this plugin manifest existed, reinstalling it is the safest way to refresh the lock-file plugin metadata.

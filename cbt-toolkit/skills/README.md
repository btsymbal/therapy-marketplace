# cbt-toolkit skills

Each skill in this plugin is a folder containing a `SKILL.md` file:

```
skills/
└── <skill-name>/
    └── SKILL.md
```

`SKILL.md` requires YAML frontmatter with two fields:

```yaml
---
name: <skill-name>
description: One line describing WHEN Claude should use this skill.
---
```

Once the plugin is installed, the skill is invoked as `/cbt-toolkit:<skill-name>`.

See `SKILL.template.md` in this directory for a copy-paste starting point.

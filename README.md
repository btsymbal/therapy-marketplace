# Therapy Plugins

A Claude Code plugin marketplace for therapy-related plugins.

## Plugins

| Plugin | Skill | Description |
| --- | --- | --- |
| `cbt-toolkit` | `/cbt-toolkit:cbt-self-help` | Interactively walks the user through evidence-based CBT techniques, one step at a time, then logs the session. |
| `act-toolkit` | `/act-toolkit:act-self-help` | Interactively walks the user through evidence-based ACT (Acceptance and Commitment Therapy) techniques, one step at a time, then logs the session. |

## Install

In Claude Code:

```
/plugin marketplace add btsymbal/therapy-marketplace
/plugin install cbt-toolkit@therapy-plugins
/plugin install act-toolkit@therapy-plugins
```

Then run `/plugin` to browse and manage installed plugins.

## Adding a skill to a plugin

Each skill lives in its own folder, `<plugin>/skills/<skill-name>/SKILL.md`,
with `name` and `description` frontmatter. Any supporting files go in a
`references/` directory beside `SKILL.md`. The skill is invoked as
`/<plugin>:<skill-name>`.

## Structure

```
therapy-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # marketplace catalog
└── <plugin>/                     # one folder per plugin (e.g. cbt-toolkit)
    ├── .claude-plugin/
    │   └── plugin.json           # plugin manifest
    └── skills/
        └── <skill-name>/         # one folder per skill
            ├── SKILL.md
            └── references/       # supporting docs
```

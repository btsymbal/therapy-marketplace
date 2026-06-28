# Therapy Plugins

A Claude Code plugin marketplace for therapy-related plugins.

## Plugins

| Plugin | Description |
| --- | --- |
| `cbt-toolkit` | Cognitive Behavioral Therapy skills and tools for Claude Code. |

## Install

In Claude Code:

```
/plugin marketplace add btsymbal/therapy-marketplace
/plugin install cbt-toolkit@therapy-plugins
```

Then run `/plugin` to browse and manage installed plugins.

## Adding a skill to a plugin

Each skill lives in `cbt-toolkit/skills/<skill-name>/SKILL.md` with `name` and
`description` frontmatter. See `cbt-toolkit/skills/README.md` for details.

## Structure

```
therapy-marketplace/
├── .claude-plugin/
│   └── marketplace.json      # marketplace catalog
└── cbt-toolkit/              # plugin
    ├── .claude-plugin/
    │   └── plugin.json       # plugin manifest
    └── skills/               # skills go here (one folder per skill)
```

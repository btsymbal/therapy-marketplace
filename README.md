# Therapy Plugins

A Claude Code plugin marketplace for therapy-related plugins.

## Plugins

| Plugin | Skill | Description |
| --- | --- | --- |
| `cbt-toolkit` | `/cbt-toolkit:cbt-self-help` | Interactively walks the user through evidence-based CBT techniques, one step at a time, then logs the session. |
| `act-toolkit` | `/act-toolkit:act-self-help` | Interactively walks the user through evidence-based ACT (Acceptance and Commitment Therapy) techniques, one step at a time, then logs the session. |
| `active-imagination-toolkit` | `/active-imagination-toolkit:active-imagination-self-help` | Interactively guides the user through Jungian active imagination across six modalities, one phase at a time, then records the session. |
| `ba-toolkit` | `/ba-toolkit:ba-self-help` | Interactively walks the user through Behavioral Activation, one step at a time, then logs the session. |
| `cft-toolkit` | `/cft-toolkit:cft-self-help` | Interactively guides the user through evidence-based Compassion-Focused Therapy practices for shame and self-criticism, one step at a time, then logs the session. |
| `dbt-toolkit` | `/dbt-toolkit:dbt-self-help` | Interactively walks the user through evidence-based DBT skills (mindfulness, distress tolerance, emotion regulation, interpersonal effectiveness), one step at a time, then logs the session. |
| `ipt-toolkit` | `/ipt-toolkit:ipt-self-help` | Interactively walks the user through evidence-based IPT techniques for grief, relationship disputes, life transitions, and isolation, one step at a time, then logs the session. |
| `mbct-toolkit` | `/mbct-toolkit:mbct-self-help` | Interactively guides the user through evidence-based MBCT mindfulness practices, one cue at a time, then logs the session. |
| `narrative-toolkit` | `/narrative-toolkit:narrative-self-help` | Interactively runs Narrative Therapy conversations — externalizing, re-authoring, re-membering — one question at a time, then logs the session. |

## Install

In Claude Code:

```
/plugin marketplace add btsymbal/therapy-marketplace
/plugin install cbt-toolkit@therapy-plugins
/plugin install act-toolkit@therapy-plugins
/plugin install active-imagination-toolkit@therapy-plugins
/plugin install ba-toolkit@therapy-plugins
/plugin install cft-toolkit@therapy-plugins
/plugin install dbt-toolkit@therapy-plugins
/plugin install ipt-toolkit@therapy-plugins
/plugin install mbct-toolkit@therapy-plugins
/plugin install narrative-toolkit@therapy-plugins
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

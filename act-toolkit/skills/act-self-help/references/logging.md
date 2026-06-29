## Contents
- Where everything lives
- Why log
- When to log
- Session start: read before you work
- Filename and location
- The entry format
- Tagging for pattern-spotting
- Cross-linking
- The CLAUDE.md memory file
- Keeping CLAUDE.md sustainable

## Where everything lives

This skill runs in Cowork. The user opens a **folder on their computer** (the working directory) and works in it; everything this skill persists goes **into that folder**, as plain markdown. There is no external database, vault, or daily-note system to wire up. Two things live in the folder:

- `act-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled context). See the last two sections.

The user will not tell you where to write or what to do with `CLAUDE.md`. Run it yourself: read the context at the start, log at the end, keep `CLAUDE.md` current. Create `act-log/` and `CLAUDE.md` the first time they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This whole persistence layer assumes a writable working directory. If there isn't one — the skill is used in plain chat, no folder was picked, or writes simply fail — treat all of it as a no-op and move on: skip the start-of-session read, skip the `act-log/` write, skip `CLAUDE.md`. Do not stall, retry in a loop, or pressure the user to pick a folder. The session runs fully without persistence; only the cross-session memory is lost. Note it once in passing if relevant, then continue normally.

## Why log

Every ACT walk-through gets filed so that across many entries the recurring stuff surfaces: which situations keep hooking the user, which thoughts they keep fusing with, which feelings they keep fighting, which values keep coming up, what actually creates room or movement. One entry is a session; the folder in aggregate is the pattern map. The log is **data, not a diary**, keep it structured and short.

## When to log

Log whenever a real walk-through happened, a technique was run, even partially, even if nothing shifted. Don't log a one-line exchange that never became a session. "Tried defusion, thought stayed sticky, willingness still low" is a valid and useful entry; a manufactured win is not.

## Session start: read before you work

At the start of a session, before routing, read the working directory's `CLAUDE.md` if it exists, that's the fast context (what to call the user, their language, recurring hooks and patterns, their named values, open commitments, crisis resources for their region). If the situation echoes something specific, skim the relevant recent file(s) in `act-log/` (e.g. `grep` the folder for the theme). Don't make the user re-explain what's already on record. If there's no `CLAUDE.md` yet, this is a first session: proceed normally and create it at the close (see below).

## Filename and location

- Path: `act-log/YYYY-MM-DD-slug.md`, inside the working directory. Create the `act-log/` folder if it doesn't exist.
- Slug: 2-4 kebab-case words naming the situation or theme, not the technique (`work-feedback-spiral`, `sunday-dread`, `craving-after-work`, `not-good-enough-story`).
- Compute the date from the system clock with `python3` / `date` (in the user's local timezone if it's recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: act-log
date: YYYY-MM-DD
presentation: <one of: anxiety | worry | panic | depression | self-criticism | stuck | grief | anger | craving | pain | other>
process: [<hexaflex processes worked, kebab-case: defusion | acceptance | present-moment | self-as-context | values | committed-action>]
techniques: [<technique names used, kebab-case>]
values: [<values named, kebab-case; omit if none surfaced>]
discomfort-before: <0-100 or 0-10>
discomfort-after: <0-100 or 0-10>
willingness-before: <0-10 or _(not noted)_>
willingness-after: <0-10 or _(not noted)_>
tags: [act, <presentation>, <process tags>, <technique tags>]
---

## Trigger

<The concrete situation, 1-2 sentences. Where, when, what happened that set it off.>

## What showed up

<The experience worked: the feeling (with intensity before -> after), the hooked thought verbatim in their words (with believability before -> after), or the urge. e.g. "The 'I'm a fraud' thought, believability 9 -> 6; anxiety 80 -> 75 but stopped fighting it.">

## What hooked / what was being fought

<In ACT terms: which inflexibility move was live — fusion with a thought, avoidance of a feeling, lost in past/future, fused to a self-story, values-drift, inaction. One line.>

## What we did

<Process and technique(s) run, in one or two lines. The key move or question that created distance / room / movement, or the one that didn't.>

## Shift

<What changed: more distance from the thought, more room for the feeling, a value named, a step taken. The re-rating (discomfort / believability / willingness), or honestly "didn't move". Plain, not inflated. Remember: less struggle with the same-sized feeling is a real result.>

## Committed action

<Any values-linked step that fell out: what, when, and the willingness rating. Or "none". One concrete thing, not a program.>
```

Keep the whole entry under ~200 words. Past that it's a reflection, not a log, file a `reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it from `Committed action`.

Never invent thoughts, feelings, values, or ratings the user didn't give. Missing data is `_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the structured tags (`presentation`, `process`, `techniques`, `values`, `tags`) in canonical English kebab-case so the folder stays greppable. But quote the **hooked thought and the user's own words verbatim in the original language**, don't translate them; the fidelity of their actual phrasing is the data.

## Tagging for pattern-spotting

`tags:` and the `presentation` / `process` / `values` frontmatter are what make the folder greppable. Always include `act` and the `presentation`. Add the processes worked, techniques used, and any values named so a later review can answer "which thoughts does the user keep fusing with", "what keeps driving the avoidance", "which value keeps coming up but isn't being lived", "does defusion or acceptance create more room for them". Don't pad with vague tags.

## Cross-linking

- If the session poked a recurring theme, the home for that is the `CLAUDE.md` "Recurring hooks & patterns" section, add or update it there rather than maintaining a separate web of links. To point at a specific past session, use a plain relative markdown link: `[sunday-dread](act-log/2026-06-21-sunday-dread.md)`.
- If the homework is a **committed action**, record its result in the next session's `Trigger` / `Shift` and update the `CLAUDE.md` "Open commitments" section; there is no separate action log.
- If it grew into a real insight, file a `reflections/` note and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short, distilled context you read at the start and refresh at the close. The session logs are the raw data; `CLAUDE.md` is the summary that makes the next session start warm instead of cold.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from any global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this one. Don't read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create it at the end of the session — always, every first session, no judgment call about whether anything was "durable enough." Fill only what you actually learned (leave the rest as placeholders, never invent). The "only if something durable changed" rule below governs *updates to an existing file*, not whether to create it. Use this template:

```markdown
# ACT self-help — working notes

Persistent memory for the act-self-help skill. Read this at the start of every
session; update it at the end per the rules at the bottom. Keep it short and true.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Familiarity with ACT: _(not noted)_
- Therapist or trusted person to reach in a crisis: _(not noted)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once
known. See references/safety.md for how to source and verify these.)_

## Values map
_(Values the user has named and how alive each is right now — the compass headings
that values/committed-action work keeps returning to. Add one once it's surfaced;
keep each to a line.)_

## Recurring hooks & patterns
_(Thoughts they keep fusing with, feelings they keep fighting, situations that keep
hooking them, and which moves create room. Add an item once it recurs; keep each to
a line.)_

## Open commitments
_(Values-linked actions set as homework, with the willingness rating and whether they
were done. Remove items once resolved.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that,
  update the sections if (and only if) something durable changed; otherwise leave
  it. Day-to-day detail stays in act-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable things go here: a fact about the user changed, a value got named, a hook recurred (a thought or trigger showing up a second or third time), a commitment was set, or a commitment was resolved.
- **Edit in place; don't append endlessly.** Promote a theme into "Recurring hooks & patterns" when it recurs; delete a commitment from "Open commitments" once it's done. The file should not grow without bound, if a section gets long, distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user", "Crisis resources (local)", and "Values map" naturally from what the session reveals; don't open with a questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if `CLAUDE.md` and the logs disagree, the logs win, reconcile during a review.
- **Reconcile on review.** During a weekly / monthly look-back, re-grep `act-log/` and update "Values map", "Recurring hooks & patterns", and "Open commitments" so the summary still matches the data.

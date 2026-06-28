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

- `cbt-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled context). See the last two sections.

The user will not tell you where to write or what to do with `CLAUDE.md`. Run it yourself: read the context at the start, log at the end, keep `CLAUDE.md` current. Create `cbt-log/` and `CLAUDE.md` the first time they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This whole persistence layer assumes a writable working directory. If there isn't one — the skill is used in plain chat, no folder was picked, or writes simply fail — treat all of it as a no-op and move on: skip the start-of-session read, skip the `cbt-log/` write, skip `CLAUDE.md`. Do not stall, retry in a loop, or pressure the user to pick a folder. The session runs fully without persistence; only the cross-session memory is lost. Note it once in passing if relevant, then continue normally.

## Why log

Every CBT walk-through gets filed so that across many entries the recurring stuff surfaces: which situations keep triggering the user, which distortions keep showing up, which thoughts keep returning, what actually shifts them. One entry is a session; the folder in aggregate is the pattern map. The log is **data, not a diary**, keep it structured and short.

## When to log

Log whenever a real walk-through happened, a technique was run, even partially, even if nothing shifted. Don't log a one-line exchange that never became a session. "Tried X, belief didn't move, flagged a possible core belief" is a valid and useful entry; a manufactured win is not.

## Session start: read before you work

At the start of a session, before routing, read the working directory's `CLAUDE.md` if it exists, that's the fast context (what to call the user, their language, recurring patterns, open homework, crisis resources for their region). If the situation echoes something specific, skim the relevant recent file(s) in `cbt-log/` (e.g. `grep` the folder for the theme). Don't make the user re-explain what's already on record. If there's no `CLAUDE.md` yet, this is a first session: proceed normally and create it at the close (see below).

## Filename and location

- Path: `cbt-log/YYYY-MM-DD-slug.md`, inside the working directory. Create the `cbt-log/` folder if it doesn't exist.
- Slug: 2-4 kebab-case words naming the situation or theme, not the technique (`work-feedback-spiral`, `sunday-dread`, `panic-on-commute`, `cold-message-freeze`).
- Compute the date from the system clock with `python3` / `date` (in the user's local timezone if it's recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: cbt-log
date: YYYY-MM-DD
problem: <one of: anxiety | worry | panic | phobia | social-anxiety | health-anxiety | depression | self-criticism | procrastination | anger | habit | sleep | pain | other>
techniques: [<technique names used, kebab-case>]
distortions: [<distortions named, kebab-case; omit if none surfaced>]
mood-before: <0-100 or 0-10>
mood-after: <0-100 or 0-10>
tags: [cbt, <problem>, <technique tags>, <distortion tags>]
---

## Trigger

<The concrete situation, 1-2 sentences. Where, when, what happened that set it off.>

## Feeling

<The emotion(s) named, with intensity before -> after. e.g. "Anxiety 80 -> 50, plus shame 60.">

## Hot thought

<The automatic thought we worked, verbatim in their words, with belief % before -> after. Circle the hottest one if there were several.>

## Distortions

<Which of the 10 were in it. Omit the section if none were named.>

## What we did

<Technique(s) run, in one or two lines. The key question that moved it, or the one that didn't.>

## Shift

<What changed: the balanced/alternative thought in their words, the re-rating, or honestly "didn't move". Plain, not inflated.>

## Follow-up

<Any homework that fell out: a behavioral experiment, an exposure rung, a worry-time slot, an activity to schedule. Or "none". One concrete thing, not a program.>
```

Keep the whole entry under ~200 words. Past that it's a reflection, not a log, file a `reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it from `Follow-up`.

Never invent thoughts, feelings, distortions, or ratings the user didn't give. Missing data is `_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the structured tags (`problem`, `techniques`, `distortions`, `tags`) in canonical English kebab-case so the folder stays greppable, use the distortion glossary mechanism in `core-toolkit.md` to map the named distortion back to its English tag. But quote the **hot thought and the user's own words verbatim in the original language**, don't translate them; the fidelity of their actual phrasing is the data.

## Tagging for pattern-spotting

`tags:` and the `problem` / `distortions` frontmatter are what make the folder greppable. Always include `cbt` and the `problem`. Add the techniques used and any distortions named so a later review can answer "how often does catastrophizing show up", "what keeps triggering the Sunday dread", "does restructuring or behavioral experiment move beliefs more". Don't pad with vague tags.

## Cross-linking

- If the session poked a recurring theme, the home for that is the `CLAUDE.md` "Recurring patterns" section, add or update it there rather than maintaining a separate web of links. To point at a specific past session, use a plain relative markdown link: `[sunday-dread](cbt-log/2026-06-21-sunday-dread.md)`.
- If the homework is an **exposure** attempt, record its result in this entry's `Follow-up`; there is no separate exposure log.
- If it grew into a real insight, file a `reflections/` note and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short, distilled context you read at the start and refresh at the close. The session logs are the raw data; `CLAUDE.md` is the summary that makes the next session start warm instead of cold.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from any global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this one. Don't read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create it at the end of the session — always, every first session, no judgment call about whether anything was "durable enough." Fill only what you actually learned (leave the rest as placeholders, never invent). The "only if something durable changed" rule below governs *updates to an existing file*, not whether to create it. Use this template:

```markdown
# CBT self-help — working notes

Persistent memory for the cbt-self-help skill. Read this at the start of every
session; update it at the end per the rules at the bottom. Keep it short and true.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Familiarity with CBT: _(not noted)_
- Therapist or trusted person to reach in a crisis: _(not noted)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once
known. See references/safety.md for how to source and verify these.)_

## Recurring patterns
_(Distortions, triggers, and themes that have shown up across more than one session.
Add an item once it recurs; keep each to a line.)_

## Open threads / homework
_(Behavioral experiments, exposure rungs, worry-time slots, activities set as
homework, and whether they were done. Remove items once resolved.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that,
  update the sections if (and only if) something durable changed; otherwise leave
  it. Day-to-day detail stays in cbt-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable things go here: a fact about the user changed, a pattern recurred (a distortion or trigger showing up a second or third time), homework was set, or homework was resolved.
- **Edit in place; don't append endlessly.** Promote a theme into "Recurring patterns" when it recurs; delete a thread from "Open threads" once it's done. The file should not grow without bound, if a section gets long, distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user" and "Crisis resources (local)" naturally from what the session reveals; don't open with a questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if `CLAUDE.md` and the logs disagree, the logs win, reconcile during a review.
- **Reconcile on review.** During a weekly / monthly look-back, re-grep `cbt-log/` and update "Recurring patterns" and "Open threads" so the summary still matches the data.

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

- `ba-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled context). See the last two sections.

The user will not tell you where to write or what to do with `CLAUDE.md`. Run it yourself: read the context at the start, log at the end, keep `CLAUDE.md` current. Create `ba-log/` and `CLAUDE.md` the first time they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This whole persistence layer assumes a writable working directory. If there isn't one — the skill is used in plain chat, no folder was picked, or writes simply fail — treat all of it as a no-op and move on: skip the start-of-session read, skip the `ba-log/` write, skip `CLAUDE.md`. Do not stall, retry in a loop, or pressure the user to pick a folder. The session runs fully without persistence; only the cross-session memory is lost. Note it once in passing if relevant, then continue normally.

## Why log

BA is a between-sessions treatment: the work is what actually gets scheduled and done, tracked across time. Every walk-through gets filed so that across many entries the mechanism becomes visible — which activities reliably lift mood, which avoidance patterns (TRAPs) keep recurring, whether predicted-vs-actual gaps are closing, whether routine is stabilizing, whether activation is climbing even when mood lags. One entry is a session; the folder in aggregate is the activation map. The log is **data, not a diary**, keep it structured and short.

## When to log

Log whenever a real walk-through happened, a technique was run, even partially, even if nothing got scheduled or nothing shifted. Don't log a one-line exchange that never became a session. "Tried to schedule, too low to commit; set wake-time anchor as the one step" is a valid and useful entry; a manufactured win is not. **If homework was set, the next session's job is to read this entry and ask what actually happened** — the follow-through is the data.

## Session start: read before you work

At the start of a session, before routing, read the working directory's `CLAUDE.md` if it exists, that's the fast context (what to call the user, their language, their values and activity menu, routine anchors, open scheduled activities and whether they got done, crisis resources for their region). If a scheduled activity from last time is open, ask how it went before scheduling anything new. If the situation echoes something specific, skim the relevant recent file(s) in `ba-log/` (e.g. `grep` the folder for the theme). Don't make the user re-explain what's already on record. If there's no `CLAUDE.md` yet, this is a first session: proceed normally and create it at the close (see below).

## Filename and location

- Path: `ba-log/YYYY-MM-DD-slug.md`, inside the working directory. Create the `ba-log/` folder if it doesn't exist.
- Slug: 2-4 kebab-case words naming the situation or theme, not the technique (`weekend-in-bed`, `stopped-seeing-friends`, `morning-routine`, `gym-avoidance`).
- Compute the date from the system clock with `python3` / `date` (in the user's local timezone if it's recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: ba-log
date: YYYY-MM-DD
problem: <one of: depression | anhedonia | grief | anxiety-overlap | pain | avoidance | routine | other>
techniques: [<technique names used, kebab-case, e.g. activity-monitoring, activity-scheduling, trap-trac, graded-task, dabbling>]
activities: [<what was actually scheduled, short kebab-case, e.g. 20-min-walk, text-a-friend, cook-dinner>]
avoidance: <the TRAP avoidance pattern in a few words, or omit if none surfaced>
mood-before: <0-100 or 0-10>
mood-after: <0-100 or 0-10>
predicted-vs-actual: <e.g. "predicted 30, actual 55" for the lift from a scheduled activity; omit if not measured>
mastery-pleasure: <e.g. "mastery 6 / pleasure 2"; omit if not rated>
tags: [ba, <problem>, <technique tags>, <activity/avoidance tags>]
---

## Context

<The concrete picture, 1-2 sentences. What a recent day/moment actually looked like, the withdrawal or avoidance in it.>

## Activity & avoidance pattern

<What's been happening behaviorally: what's dropped off, the avoidance cycle. If a TRAP was mapped, note Trigger -> Response -> Avoidance Pattern in their words.>

## What we scheduled / did

<Technique(s) run, in one or two lines. The specific activity scheduled (what / when / where), or the TRAC alternative built. The key step that moved it, or the one that didn't.>

## Mastery & pleasure

<Predicted vs actual mood for the activity, and mastery/pleasure scores if taken. Name the underprediction gap if there was one. Omit the section if nothing was scored.>

## Shift

<What changed: mood re-rating, whether activation moved even if mood lagged, the user's own words. Plain, not inflated. "Didn't move but she did the walk" is honest and useful.>

## Follow-up

<The one concrete next thing: the activity scheduled before next time (with day/time), or the TRAC plan for when the trigger next hits. Or "none". One concrete thing, not a program.>
```

Keep the whole entry under ~200 words. Past that it's a reflection, not a log, file a `reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it from `Follow-up`.

Never invent activities, moods, scores, or avoidance patterns the user didn't give. Missing data is `_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the structured tags (`problem`, `techniques`, `activities`, `tags`) in canonical English kebab-case so the folder stays greppable. But quote the **user's own words verbatim in the original language** in the body — their actual phrasing of the trigger, the avoidance, what they noticed — don't translate them; the fidelity is the data.

## Tagging for pattern-spotting

`tags:` and the `problem` / `techniques` / `activities` / `avoidance` frontmatter are what make the folder greppable. Always include `ba` and the `problem`. Add the techniques used, the activities scheduled, and the avoidance pattern so a later review can answer "which activities actually lift her mood", "does the gym avoidance keep recurring", "is the predicted-vs-actual gap closing", "is activation climbing across the month". Don't pad with vague tags.

## Cross-linking

- If the session poked a recurring pattern, the home for that is the `CLAUDE.md` "Recurring patterns" section, add or update it there rather than maintaining a separate web of links. To point at a specific past session, use a plain relative markdown link: `[gym-avoidance](ba-log/2026-06-21-gym-avoidance.md)`.
- If the homework is a **scheduled activity** or a **TRAC plan**, record its result in the *next* entry's `Context`/`Shift`; the open item lives in `CLAUDE.md` "Open threads" until it's done.
- If it grew into a real insight, file a `reflections/` note and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short, distilled context you read at the start and refresh at the close. The session logs are the raw data; `CLAUDE.md` is the summary that makes the next session start warm — already knowing what the user values, which activities have worked, and what's scheduled.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from any global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this one. Don't read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create it at the end of the session — always, every first session, no judgment call about whether anything was "durable enough." Fill only what you actually learned (leave the rest as placeholders, never invent). The "only if something durable changed" rule below governs *updates to an existing file*, not whether to create it. Use this template:

```markdown
# Behavioral Activation — working notes

Persistent memory for the ba-self-help skill. Read this at the start of every
session; update it at the end per the rules at the bottom. Keep it short and true.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Familiarity with BA: _(not noted)_
- Therapist or trusted person to reach in a crisis: _(not noted)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once
known. See references/safety.md for how to source and verify these.)_

## Values & life areas
_(What matters to them that's gone quiet — the gaps between what they care about and
what they're currently doing. The targets BA scheduling pulls toward. One line each.)_

## Activity menu
_(Small activities that have actually helped, sorted by energy: very-low-energy days
(make tea, sit outside 5 min) vs moderate-energy days (text a friend, cook a meal).
Built up from what the logs show works for THIS person, not a generic pleasant-events list.)_

## Routine anchors
_(The structural points being stabilized — consistent wake-time first of all, then
meals, then layered activities. Note what's holding and what isn't.)_

## Open threads / scheduled activities
_(Activities and TRAC plans set as homework, with day/time, and whether they got done.
Remove an item once it's resolved; carry it forward if it didn't happen and ask why.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that,
  update the sections if (and only if) something durable changed; otherwise leave
  it. Day-to-day detail stays in ba-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable things go here: a fact about the user changed, a value surfaced, an activity proved it reliably helps (or reliably doesn't), a routine anchor held or broke, homework was set, or homework was resolved.
- **Edit in place; don't append endlessly.** Promote an activity into the "Activity menu" once it's shown it works; move a recurring avoidance into "Recurring patterns" (add the section if needed); delete a scheduled item from "Open threads" once it's done. The file should not grow without bound, if a section gets long, distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user", "Values & life areas", and "Crisis resources" naturally from what the session reveals; don't open with a questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if `CLAUDE.md` and the logs disagree, the logs win, reconcile during a review.
- **Reconcile on review.** During a weekly / monthly look-back, re-grep `ba-log/` and update "Activity menu", "Routine anchors", and "Open threads" so the summary still matches the data.

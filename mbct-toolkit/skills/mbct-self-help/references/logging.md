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

This skill runs in Cowork. The user opens a **folder on their computer** (the working
directory) and works in it; everything this skill persists goes **into that folder**, as plain
markdown. There is no external database or vault to wire up. Two things live in the folder:

- `mbct-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled context). See the
  last two sections.

The user will not tell you where to write or what to do with `CLAUDE.md`. Run it yourself: read
the context at the start, log at the end, keep `CLAUDE.md` current. Create `mbct-log/` and
`CLAUDE.md` the first time they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This whole persistence layer assumes a writable working
directory. If there isn't one — plain chat, no folder picked, or writes fail — treat all of it
as a no-op and move on: skip the start-of-session read, skip the `mbct-log/` write, skip
`CLAUDE.md`. Do not stall, retry in a loop, or pressure the user to pick a folder. The session
runs fully without persistence; only the cross-session memory is lost. Note it once in passing
if relevant, then continue normally.

## Why log

Every MBCT walk-through gets filed so that across many entries the recurring stuff surfaces:
which situations keep pulling the user into autopilot or rumination, which practices actually
help them decenter, what their personal relapse signature looks like as it accumulates, and
whether the *relationship* to difficulty is shifting over time even when feelings don't. One
entry is a session; the folder in aggregate is the pattern map — and for relapse prevention,
that map *is* the early-warning system. The log is **data, not a diary**; keep it structured
and short.

## When to log

Log whenever a real walk-through happened — a practice was guided, even partially, even if
nothing shifted or the mind stayed busy. Don't log a one-line exchange that never became a
session. "Tried allowing, distress spiked, stepped back to breath, stayed caught" is a valid
and useful entry; a manufactured calm is not.

## Session start: read before you work

At the start of a session, before routing, read the working directory's `CLAUDE.md` if it
exists — that's the fast context (what to call the user, their language, recurring patterns,
their relapse signature, established practice slots, crisis resources for their region). If the
situation echoes something specific, skim the relevant recent file(s) in `mbct-log/` (e.g.
`grep` the folder for the theme). Don't make the user re-explain what's already on record. If
there's no `CLAUDE.md` yet, this is a first session: proceed normally and create it at the close.

## Filename and location

- Path: `mbct-log/YYYY-MM-DD-slug.md`, inside the working directory. Create `mbct-log/` if it
  doesn't exist.
- Slug: 2-4 kebab-case words naming the situation or theme, not the practice
  (`replaying-the-meeting`, `sunday-low`, `2am-cant-switch-off`, `worry-spiral-before-flight`).
- Compute the date from the system clock with `python3` / `date` (in the user's local timezone
  if it's recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: mbct-log
date: YYYY-MM-DD
problem: <one of: rumination | depression | worry | panic | social-anxiety | health-anxiety | autopilot | pain | insomnia | craving | other>
practices: [<practice names used, kebab-case; e.g. breath-sitting, sounds-and-thoughts, allowing, breathing-space>]
mood-before: <0-100 or 0-10>
mood-after: <0-100 or 0-10>
decentering-before: <how caught/fused — 0-100 "feels like a fact", or a phrase like "fully inside it">
decentering-after: <same scale/phrase after — the relationship shift>
tags: [mbct, <problem>, <practice tags>]
---

## Trigger

<The concrete situation, 1-2 sentences. Where, when, what set it off.>

## Feeling

<The emotion(s) named, with intensity before -> after. e.g. "Anxiety 75 -> 55, plus self-criticism.">

## What was caught

<The thought / sensation / mood they were fused to, verbatim in their words, with the
caught -> observing shift. e.g. "'I always ruin these things' — from total fact to 'a thought I'm
having'." This is MBCT's core measure; capture it even when distress didn't move.>

## What we did

<Practice(s) guided, in one or two lines. Which anchor, whether we turned toward anything, how
the mind behaved (busy, drowsy, settled). The cue that helped, or the one that didn't.>

## Shift

<What changed: the change in relationship to the experience and/or the re-rating, in their
words. Plain, not inflated. "Relationship shifted, feeling stayed" is a real result; so is
"stayed caught".>

## Follow-up

<Any between-session practice that fell out: a daily 3MBS slot, a body scan, a pleasant-events
note, a line added to their relapse signature. Or "none". One concrete thing, not a program.>
```

Keep the whole entry under ~200 words. Past that it's a reflection, not a log; file a
`reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it from
`Follow-up`.

Never invent thoughts, feelings, or ratings the user didn't give. Missing data is
`_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the structured
tags (`problem`, `practices`, `tags`) in canonical English kebab-case so the folder stays
greppable. But quote the **caught thought and the user's own words verbatim in the original
language** — the fidelity of their actual phrasing is the data.

## Tagging for pattern-spotting

`tags:`, `problem`, and `practices` are what make the folder greppable. Always include `mbct`
and the `problem`. Add the practices used so a later review can answer "what keeps pulling them
into rumination", "does the body scan or the breathing space settle them more", "is the
caught → observing shift getting easier over time". Don't pad with vague tags.

## Cross-linking

- If the session poked a recurring theme or added to the user's relapse signature, the home for
  that is the `CLAUDE.md` "Recurring patterns" / "Relapse signature" sections — update there
  rather than maintaining a separate web of links. To point at a specific past session, use a
  plain relative markdown link: `[sunday-low](mbct-log/2026-06-21-sunday-low.md)`.
- If it grew into a real insight, file a `reflections/` note and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short, distilled
context you read at the start and refresh at the close. The session logs are the raw data;
`CLAUDE.md` is the summary that makes the next session start warm instead of cold — and, for
relapse prevention, the place the user's early-warning signature accumulates.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from any
global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this one.
Don't read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create it at
the end of the session — always, every first session, no judgment call about whether anything
was "durable enough." Fill only what you actually learned (leave the rest as placeholders, never
invent). The "only if something durable changed" rule below governs *updates to an existing
file*, not whether to create it. Use this template:

```markdown
# MBCT self-help — working notes

Persistent memory for the mbct-self-help skill. Read this at the start of every session;
update it at the end per the rules at the bottom. Keep it short and true.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Familiarity with mindfulness / MBCT: _(not noted)_
- Relevant history (e.g. recurrent depression — for relapse-prevention framing): _(not noted)_
- Therapist or trusted person to reach in a crisis: _(not noted)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once known.
See references/safety.md for how to source and verify these.)_

## Relapse signature / early-warning signs
_(The user's personal early-warning signs and characteristic automatic thoughts, accumulated
across sessions — earliest-appearing to most-advanced. The heart of MBCT relapse prevention;
build it up over time. See core-practices.md 1.15.)_

## Recurring patterns
_(Triggers, autopilot situations, and themes that have shown up across more than one session,
plus which practices help this user decenter. Add an item once it recurs; keep each to a line.)_

## Established practice / open threads
_(Daily practice slots set (e.g. 3MBS on waking + before bed), body-scan habit, and any
between-session practice set as homework and whether it was done. Remove items once resolved.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that, update the
  sections if (and only if) something durable changed; otherwise leave it. Day-to-day detail
  stays in mbct-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable
  things go here: a fact about the user changed, a pattern recurred, a sign was added to the
  relapse signature, a practice habit was established or dropped.
- **Edit in place; don't append endlessly.** Promote a theme into "Recurring patterns" when it
  recurs; add to "Relapse signature" as signs are identified; remove a thread from "Established
  practice / open threads" once it's resolved or abandoned. The file should not grow without
  bound; if a section gets long, distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user" and "Crisis
  resources (local)" naturally from what the session reveals; don't open with a questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if `CLAUDE.md`
  and the logs disagree, the logs win — reconcile during a review.
- **Reconcile on review.** During a weekly / monthly look-back, re-grep `mbct-log/` and update
  "Recurring patterns", "Relapse signature", and "Established practice" so the summary still
  matches the data.

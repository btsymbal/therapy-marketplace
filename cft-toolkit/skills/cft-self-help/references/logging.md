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
directory) and works in it; everything this skill persists goes **into that folder**, as
plain markdown. There is no external database or vault to wire up. Two things live in the
folder:

- `cft-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled context).
  See the last two sections.

The user will not tell you where to write or what to do with `CLAUDE.md`. Run it yourself:
read the context at the start, log at the end, keep `CLAUDE.md` current. Create `cft-log/`
and `CLAUDE.md` the first time they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This whole persistence layer assumes a writable
working directory. If there isn't one — the skill is used in plain chat, no folder was
picked, or writes simply fail — treat all of it as a no-op and move on: skip the
start-of-session read, skip the `cft-log/` write, skip `CLAUDE.md`. Do not stall, retry in
a loop, or pressure the user to pick a folder. The session runs fully without persistence;
only the cross-session memory is lost. Note it once in passing if relevant, then continue.

## Why log

Every practice gets filed so that across many entries the recurring stuff surfaces: which
situations keep activating the threat system, where shame keeps attaching, which compassion
flow stays blocked, which fears/blocks/resistances (FBRs) keep showing up, what actually
builds soothing access, and how far up the ladder the user can reliably go. One entry is a
session; the folder in aggregate is the map. The log is **data, not a diary** — keep it
structured and short.

## When to log

Log whenever a real practice happened — a technique was guided, even partially, even if
nothing shifted, even if backdraft cut it short. Don't log a one-line exchange that never
became a session. "Tried safe place, backdraft surfaced, dropped to SRB, flagged fear of
self-compassion" is a valid and useful entry; a manufactured calm is not.

## Session start: read before you work

At the start of a session, before routing, read the working directory's `CLAUDE.md` if it
exists — that's the fast context (what to call the user, their language, which flows are
blocked, recurring FBRs, where on the ladder they reached last time, open practice, crisis
resources for their region). If the situation echoes something specific, skim the relevant
recent file(s) in `cft-log/`. Don't make the user re-explain what's already on record. If
there's no `CLAUDE.md` yet, this is a first session: proceed normally and create it at the
close (see below).

## Filename and location

- Path: `cft-log/YYYY-MM-DD-slug.md`, inside the working directory. Create the `cft-log/`
  folder if it doesn't exist.
- Slug: 2-4 kebab-case words naming the situation or theme, not the technique
  (`work-feedback-shame`, `cant-receive-kindness`, `inner-critic-spiral`,
  `presentation-dread`).
- Compute the date from the system clock with `python3` / `date` (in the user's local
  timezone if it's recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: cft-log
date: YYYY-MM-DD
problem: <one of: shame | self-criticism | depression | anxiety | social-anxiety | ocd | trauma-shame | anger | perfectionism | pain | other>
techniques: [<CMT practice names used, kebab-case, e.g. srb, safe-place, compassionate-self, compassionate-letter>]
flows-blocked: [<self-to-others | others-to-self | self-to-self; omit if none surfaced>]
fbr: <yes | no — was a fear/block/resistance to compassion present?>
distress-before: <0-100 or 0-10>
distress-after: <0-100 or 0-10>
soothing-before: <0-100 — felt warmth/safeness access>
soothing-after: <0-100>
ladder-reached: <psychoeducation | srb | safe-place | compassionate-other | compassionate-self | letter | none>
tags: [cft, <problem>, <technique tags>, <flow/fbr tags>]
---

## Trigger

<The concrete situation, 1-2 sentences. Where, when, what set it off.>

## Threat & shame

<What the threat system was reacting to, and the self-criticism / shame in it: the inner
voice verbatim in their words if there was one, and whether it was external shame (how
others see me) or internal shame (what I am). Intensity before -> after.>

## What we did

<Practice(s) guided, in one or two lines. The cue that opened something, or the one that
triggered backdraft. Where on the ladder we got to.>

## What shifted

<Soothing/safeness access before -> after, in their words ("a warm spot in the chest that
wasn't there"), and any change in the shame/threat. Plain, not inflated; "didn't shift,
backdraft surfaced" is valid. Note which flow opened or stayed blocked.>

## Follow-up

<Any between-session practice: a daily SRB slot, a safe-place rehearsal, a compassionate
letter to finish. Or "none". One concrete thing, not a program.>
```

Keep the whole entry under ~200 words. Past that it's a reflection, not a log — file a
`reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it
from `Follow-up`.

Never invent feelings, images, flows, FBRs, or ratings the user didn't give. Missing data
is `_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the
structured tags (`problem`, `techniques`, `flows-blocked`, `tags`) in canonical English
kebab-case so the folder stays greppable. But quote the **inner-critic voice and the user's
own words verbatim in the original language** — the fidelity of their actual phrasing is the
data.

## Tagging for pattern-spotting

`tags:` and the `problem` / `flows-blocked` / `fbr` frontmatter are what make the folder
greppable. Always include `cft` and the `problem`. Add the practices used, which flow was
blocked, and whether an FBR was present, so a later review can answer "is self-compassion
still the blocked flow", "does backdraft keep cutting safe-place work short", "what builds
soothing access fastest for them". Don't pad with vague tags.

## Cross-linking

- If the session poked a recurring theme (a flow that stays blocked, an FBR that keeps
  recurring, a trigger that keeps firing), the home for that is the `CLAUDE.md` "Flows,
  FBRs & patterns" section — add or update it there rather than maintaining a web of links.
  To point at a specific past session, use a plain relative markdown link:
  `[cant-receive-kindness](cft-log/2026-06-21-cant-receive-kindness.md)`.
- If it grew into a real insight, file a `reflections/` note and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short,
distilled context you read at the start and refresh at the close. The session logs are the
raw data; `CLAUDE.md` is the summary that makes the next session start warm instead of cold.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from
any global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this
one. Don't read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create
it at the end of the session — always, every first session, no judgment call about whether
anything was "durable enough." Fill only what you actually learned (leave the rest as
placeholders, never invent). The "only if something durable changed" rule below governs
*updates to an existing file*, not whether to create it. Use this template:

```markdown
# CFT self-help — working notes

Persistent memory for the cft-self-help skill. Read this at the start of every
session; update it at the end per the rules at the bottom. Keep it short and true.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Familiarity with CFT / compassion practices: _(not noted)_
- Therapist or trusted person to reach in a crisis: _(not noted)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once
known. See references/safety.md for how to source and verify these.)_

## Flows, FBRs & patterns
_(Which compassion flow is blocked (to others / receiving / to self), which
fears/blocks/resistances to compassion recur, and the threats/shame themes that show up
across more than one session. Add an item once it recurs; keep each to a line.)_

## Where on the ladder
_(The highest rung reliably reached — psychoeducation, SRB, safe place, compassionate
other, compassionate self, letter — and what tends to trigger backdraft. Update as it
moves.)_

## Open threads / practice
_(Between-session practice set — daily SRB, safe-place rehearsal, a letter to finish — and
whether it was done. Remove items once resolved.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that,
  update the sections if (and only if) something durable changed; otherwise leave
  it. Day-to-day detail stays in cft-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable
  things go here: a fact about the user changed, a flow stayed blocked across sessions, an
  FBR recurred, the ladder moved, practice was set or resolved.
- **Edit in place; don't append endlessly.** Promote a theme into "Flows, FBRs & patterns"
  when it recurs; delete a thread from "Open threads" once it's done. If a section grows,
  distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user" and "Crisis
  resources (local)" naturally from what the session reveals; don't open with a
  questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if
  `CLAUDE.md` and the logs disagree, the logs win — reconcile during a review.
- **Reconcile on review.** During a weekly / monthly look-back, re-grep `cft-log/` and
  update "Flows, FBRs & patterns", "Where on the ladder", and "Open threads" so the summary
  still matches the data.

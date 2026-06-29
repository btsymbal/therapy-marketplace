## Contents
- Where everything lives
- Why record
- When to log
- Session start: read before you work
- Filename and location
- The entry format
- Tagging for pattern-spotting
- Cross-linking and the series
- The CLAUDE.md memory file
- Keeping CLAUDE.md sustainable

## Where everything lives

This skill runs in Cowork. The user opens a **folder on their computer** (the working directory)
and works in it; everything this skill persists goes **into that folder**, as plain markdown.
There is no external vault or daily-note system to wire up. Two things live in the folder:

- `active-imagination-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled context).

The user won't tell you where to write. Run it yourself: read the context at the start, record at
the end, keep `CLAUDE.md` current. Create `active-imagination-log/` and `CLAUDE.md` the first time
they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This persistence layer assumes a writable working
directory. If there isn't one — plain chat, no folder picked, or writes fail — treat all of it as
a no-op and move on: skip the start read, skip the log write, skip `CLAUDE.md`. Don't stall, retry
in a loop, or pressure the user. The session runs fully without persistence; only cross-session
memory is lost. Note it once in passing if relevant, then continue.

## Why record

Recording is part of the active-imagination method, not bookkeeping bolted on. Jung directed it
explicitly and modeled it in the Black Books: dated, systematic recording **protects the material
from fading back into the unconscious**, makes it available for later reflection (with an analyst,
if there is one), and **initiates the Phase 4 ethical confrontation** by objectifying what was
experienced (`foundations.md`). Across many entries the recurring stuff surfaces: which figures
keep returning, which images repeat, where dual awareness wobbles, what each encounter obliged and
whether it was acted on. One entry is a session; the folder in aggregate is the individuation
thread. Keep it **data, not a diary** — structured and short.

## When to log

Log whenever a real walkthrough happened — a modality was entered, even partially, even if nothing
arose, even if it ended early on the exit protocol. Don't log a one-line exchange that never became
a session. "Held the feeling-state, no image formed, grounded fine" is a valid, useful entry; a
manufactured encounter is not. **An early exit on a failure mode should be logged plainly** (what
happened, that grounding was used) — that's important safety data for the next session.

## Session start: read before you work

At the start, before routing, read the working directory's `CLAUDE.md` if it exists — the fast
context (what to call the user, their language, recurring figures/themes, the standing ethical
commitments not yet acted on, crisis resources for their region, any readiness flags). If the
material echoes something specific, skim the relevant recent file(s) in `active-imagination-log/`
(e.g. `grep` the folder for a figure's name or a theme). Don't make the user re-explain what's on
record. No `CLAUDE.md` yet means a first session: proceed and create it at the close.

## Filename and location

- Path: `active-imagination-log/YYYY-MM-DD-slug.md`, inside the working directory. Create the
  folder if it doesn't exist.
- Slug: 2-4 kebab-case words naming the figure, image, or theme (`the-old-man-at-the-gate`,
  `recurring-flood-dream`, `shadow-in-the-cellar`, `mandala-check-in`), not the modality.
- Compute the date from the system clock with `python3` / `date` (in the user's local timezone if
  recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: active-imagination-log
date: YYYY-MM-DD
modality: <one of: visual | writing | movement | art | music | sandplay>
entry-point: <dream-image | inner-figure | mood-state | recurring-dream | other>
indication: <one of: individuation | shadow | anima-animus | dream-drought | recurring-dream | spiritual-emergency | creative-block | depression | anxiety | grief | trauma | other>
phase-reached: <1-emptying | 2-surfacing | 3-engagement | 4-integration | exited-early>
dual-awareness: <held | wobbled | collapsed-exited>
figures: [<names/descriptors of figures encountered, kebab-case; omit if none>]
mood-before: <0-100 or 0-10>
mood-after: <0-100 or 0-10>
tags: [active-imagination, <modality>, <indication>, <figure tags>]
---

## Entry point

<What we started from: the image, figure, or mood, and how it was reached. 1-2 sentences.>

## What arose

<The figures, images, or impulses that came up — and what surprised them (the genuine-material
test). Quote the figure's words and the user's words verbatim, in the original language.>

## The exchange

<What was asked, what answered, where it went. The key open question that opened something, or
the moment it stalled / had to stop. One or two lines.>

## Dual awareness

<Held / wobbled / collapsed. If it wobbled or collapsed, what happened and that grounding was
used. Omit only if unremarkable and held throughout.>

## Ethical confrontation (Phase 4)

<What the material obliged — the one concrete act they committed to, in their words. If the
session exited early and Phase 4 wasn't reached, say so.>

## Follow-up

<The standing commitment to carry, a between-session practice (a daily inviting-the-image slot, a
mandala series to continue, a dialogue to resume), or "none". One concrete thing, not a program.>
```

Keep the whole entry under ~250 words. Past that it's a reflection, not a log — file a
`reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it from
`Follow-up`. Never invent figures, words, images, or ratings the user didn't give. Missing data is
`_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the structured
frontmatter tags (`modality`, `indication`, `phase-reached`, `dual-awareness`, `tags`) in
canonical English kebab-case so the folder stays greppable, but quote the **figures' and the
user's actual words verbatim in the original language** — the fidelity of the exact phrasing is the
data.

## Tagging for pattern-spotting

`tags:` and the frontmatter (`modality`, `indication`, `figures`, `dual-awareness`) make the folder
greppable. Always include `active-imagination`, the modality, and the indication. Add figure tags
so a later review can answer "how often does the old man return", "does the flood dream keep
recurring", "where does dual awareness tend to wobble". Don't pad with vague tags.

## Cross-linking and the series

- A recurring figure or theme belongs in the `CLAUDE.md` "Recurring figures & themes" section —
  add or update it there rather than maintaining a web of links. To point at a specific past
  session, use a plain relative link: `[the-old-man](active-imagination-log/2026-06-21-the-old-man-at-the-gate.md)`.
- **Series tracking is a real Jungian method.** Mandala series and sandplay series are meant to be
  reviewed across time. If the user is building one, note the series in `CLAUDE.md` "Open threads"
  and link the entries so the progression can be followed.
- If a session grew into a genuine insight, file a `reflections/` note and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short, distilled
context you read at the start and refresh at the close. The session logs are the raw data;
`CLAUDE.md` is the summary that makes the next session start warm instead of cold.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from any
global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this one. Don't
read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create it at
the end of the session — always, every first session. Fill only what you actually learned (leave
the rest as placeholders, never invent). The "only if something durable changed" rule below governs
*updates to an existing file*, not whether to create it. Template:

```markdown
# Active imagination self-help — working notes

Persistent memory for the active-imagination-self-help skill. Read this at the start of every
session; update it at the end per the rules at the bottom. Keep it short and true.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Familiarity with active imagination / Jungian work: _(not noted)_
- Working with an analyst / therapist? (and who to reach in a crisis): _(not noted)_

## Readiness & safety flags
_(Anything bearing on whether self-practice is appropriate: stable life structure or not, any
contraindication that's been raised, history of dissolution/inflation, modalities to avoid. See
references/safety.md. If a contraindication is present, this is recorded here and routing must
respect it.)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once known. See
references/safety.md for how to source and verify these.)_

## Recurring figures & themes
_(Inner figures, images, and themes that have shown up across more than one session. Add an item
once it recurs; keep each to a line.)_

## Open threads / standing commitments
_(Phase-4 ethical commitments not yet acted on, mandala/sandplay series in progress, dialogues to
resume, between-session practices set. Remove items once resolved.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that, update the
  sections only if something durable changed; otherwise leave it. Day-to-day detail stays in
  active-imagination-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable things
  go here: a fact about the user changed, a figure/theme recurred, a readiness or safety flag was
  raised, a standing commitment was set or resolved.
- **Edit in place; don't append endlessly.** Promote a figure into "Recurring figures & themes"
  when it returns; delete a thread from "Open threads" once it's done. If a section gets long,
  distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user" and the resources
  naturally from what the session reveals; don't open with a questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if `CLAUDE.md`
  and the logs disagree, the logs win — reconcile during a review.
- **Reconcile on review.** During a periodic look-back, re-grep `active-imagination-log/` and
  update "Recurring figures & themes" and "Open threads" so the summary still matches the data.

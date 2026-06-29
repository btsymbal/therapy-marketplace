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

- `narrative-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled story-so-far). See the last two sections.

The user will not tell you where to write or what to do with `CLAUDE.md`. Run it yourself: read the context at the start, log at the end, keep `CLAUDE.md` current. Create `narrative-log/` and `CLAUDE.md` the first time they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This whole persistence layer assumes a writable working directory. If there isn't one — the skill is used in plain chat, no folder was picked, or writes simply fail — treat all of it as a no-op and move on: skip the start-of-session read, skip the `narrative-log/` write, skip `CLAUDE.md`. Do not stall, retry in a loop, or pressure the user to pick a folder. The conversation runs fully without persistence; only the cross-session memory is lost. Note it once in passing if relevant, then continue normally.

## Why log

Narrative Therapy builds the alternative story **across sessions** — a single unique outcome is thin; the power comes from linking many of them into a history with a theme. The log is what makes that possible: each entry records the externalized problem, the dominant story, the unique outcomes found, and the preferred story as it thickens, so that over many entries the preferred account has a documented history the user can re-read and stand on. The log is **data, not a diary** — keep it structured and short. (Re-reading documents is itself a narrative practice: people "remake and re-experience the meaning located in them".)

## When to log

Log whenever a real conversation happened, a map was worked, even partially, even if nothing shifted. Don't log a one-line exchange that never became a session. "Externalized the Worry, no unique outcome yet, flagged Absent-But-Implicit for next time" is a valid and useful entry; a manufactured breakthrough is not.

## Session start: read before you work

At the start of a session, before routing, read the working directory's `CLAUDE.md` if it exists — that's the fast context (what to call the user, their language, the names they've given problems, the dominant and preferred stories so far, club-of-life members, open documents, crisis resources for their region). If the situation echoes a specific past thread, skim the relevant recent file(s) in `narrative-log/` (e.g. `grep` the folder for the externalized name or theme). Don't make the user re-tell what's already on record — pick the story up where it was left. If there's no `CLAUDE.md` yet, this is a first session: proceed normally and create it at the close (see below).

## Filename and location

- Path: `narrative-log/YYYY-MM-DD-slug.md`, inside the working directory. Create the `narrative-log/` folder if it doesn't exist.
- Slug: 2-4 kebab-case words naming the situation or theme, not the map (`fraud-at-work`, `carrying-my-dad`, `the-worry-runs-mornings`, `keys-back-from-ms`).
- Compute the date from the system clock with `python3` / `date` (in the user's local timezone if it's recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: narrative-log
date: YYYY-MM-DD
domain: <one of: depression | anxiety | trauma | eating-disorder | grief | child-adolescent | relationships | substance-use | chronic-illness | identity | other>
maps: [<maps/tools used, kebab-case: externalizing | re-authoring | re-membering | definitional-ceremony | unique-outcomes | scaffolding | absent-but-implicit | documents>]
externalized-name: "<the user's own name for the problem, verbatim; omit if none yet>"
unique-outcomes: <count this session, or 0>
tags: [narrative, <domain>, <map tags>]
---

## Problem-saturated story

<The dominant story the user arrived with, 1-2 sentences, in their words where possible. The account the problem has recruited them into.>

## The problem, externalized

<The name negotiated for the problem and what it does at its strongest. Its effects on their life/relationships/identity (relative-influence). e.g. "'the Fog' — flattens mornings, tells them they're a burden.">

## Unique outcomes

<Any exception(s) to the problem's story found this session — a moment the problem didn't win, an intention, a feeling, a connection. Verbatim where it matters. Or "none surfaced yet".>

## Preferred story / identity claim

<What came into view about who they prefer to be — the value, the alternative account, the title they gave it if any. In their words. Plain, not inflated.>

## Re-membered figures

<Any "club of life" members named — who they contributed to the user's life and what they'd say about this. Omit if none.>

## Next step

<Any document, witness, re-membering, or move that fell out: a letter to write, a counter-document, someone to share the story with, a person to re-member. Or "none". One concrete thing.>
```

Keep the whole entry under ~200 words. Past that it's a reflection, not a log — file a `reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it from `Next step`.

Never invent names, exceptions, figures, or meanings the user didn't give. Missing data is `_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the structured tags (`domain`, `maps`, `tags`) in canonical English kebab-case so the folder stays greppable. But quote the **externalized name, the preferred-story title, and the user's own words verbatim in the original language** — in Narrative Therapy the person's exact phrasing is the data; don't translate it.

## Tagging for pattern-spotting

`tags:`, `domain`, and `maps` are what make the folder greppable. Always include `narrative` and the `domain`. Add the maps/tools used so a later review can answer "how is the preferred story thickening", "which unique outcomes keep recurring", "what does the Fear's history look like across sessions". Don't pad with vague tags.

## Cross-linking

- If the session thickened a recurring preferred story, its home is the `CLAUDE.md` "Preferred story" section — update it there rather than maintaining a separate web of links. To point at a specific past session, use a plain relative markdown link: `[carrying-my-dad](narrative-log/2026-06-21-carrying-my-dad.md)`.
- If a unique outcome links to earlier ones, note the connection in this entry's "Preferred story" — that *is* the re-authoring history accumulating.
- If it grew into a real insight or a finished document, file it (`reflections/` or a named document file) and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short, distilled story-so-far you read at the start and refresh at the close. The session logs are the raw data; `CLAUDE.md` is the summary that lets the next session continue the alternative story instead of restarting it.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from any global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this one. Don't read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create it at the end of the session — always, every first session, no judgment call about whether anything was "durable enough". Fill only what you actually learned (leave the rest as placeholders, never invent). The "only if something durable changed" rule below governs *updates to an existing file*, not whether to create it. Use this template:

```markdown
# Narrative self-help — working notes

Persistent memory for the narrative-self-help skill. Read this at the start of every
session; update it at the end per the rules at the bottom. Keep it short and true.
This is where the preferred story is kept alive between sessions.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Therapist or trusted person to reach in a crisis: _(not noted)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once
known. See references/safety.md for how to source and verify these.)_

## Externalized problems
_(The names the user has given problems and what each does — "the Fog", "the What-If
Spiral", "Ana". One line each. These names are theirs; keep them verbatim.)_

## Dominant story → preferred story
_(The problem-saturated account they arrived with, and the alternative/preferred account
as it thickens across sessions, with its title once they name it. This is the spine of
the work — update it as unique outcomes accumulate.)_

## Club of life
_(Significant figures re-membered — living, deceased, or fictional — who support the
preferred identity, and what each contributes. Add as they're named.)_

## Documents made
_(Letters, certificates, counter-documents, a Tree of Life — what exists and where, so
they can be re-read and built on.)_

## Open threads / next steps
_(A letter to write, a witness to share with, a person to re-member, a counter-document
to make. Remove items once done.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that, update
  the sections if (and only if) something durable changed; otherwise leave it. Day-to-day
  detail stays in narrative-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable things go here: a fact about the user changed, a new externalized name was negotiated, the preferred story thickened, a club-of-life member was added, a document was made, a next step was set or done.
- **Edit in place; don't append endlessly.** Promote the preferred story as it develops; delete a thread from "Open threads" once it's done. The file should not grow without bound — if a section gets long, distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user" and "Crisis resources (local)" naturally from what the session reveals; don't open with a questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if `CLAUDE.md` and the logs disagree, the logs win — reconcile during a review.
- **Reconcile on review.** During a weekly / monthly look-back, re-grep `narrative-log/` and update "Dominant story → preferred story" and "Open threads" so the summary still matches the data.

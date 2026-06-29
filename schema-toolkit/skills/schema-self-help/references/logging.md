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

- `schema-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled context, including the user's schema profile and mode map). See the last two sections.

The user will not tell you where to write or what to do with `CLAUDE.md`. Run it yourself: read the context at the start, log at the end, keep `CLAUDE.md` current. Create `schema-log/` and `CLAUDE.md` the first time they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This whole persistence layer assumes a writable working directory. If there isn't one — the skill is used in plain chat, no folder was picked, or writes simply fail — treat all of it as a no-op and move on: skip the start-of-session read, skip the `schema-log/` write, skip `CLAUDE.md`. Do not stall, retry in a loop, or pressure the user to pick a folder. The session runs fully without persistence; only the cross-session memory is lost. Note it once in passing if relevant, then continue normally.

## Why log

Every schema walk-through gets filed so that across many entries the recurring stuff surfaces: which schemas keep firing, which modes the user flips into, which triggers and relationship dynamics keep replaying, what actually softens the charge. One entry is a session; the folder in aggregate is the schema/mode map of a life. The log is **data, not a diary**, keep it structured and short.

## When to log

Log whenever a real walk-through happened, a technique was run, even partially, even if nothing shifted. Don't log a one-line exchange that never became a session. "Tried imagery, Detached Protector blocked access, flagged for next time" is a valid and useful entry; a manufactured win is not.

## Session start: read before you work

At the start of a session, before routing, read the working directory's `CLAUDE.md` if it exists, that's the fast context (what to call the user, their language, their schema profile, their mode map and typical mode sequences, open homework, crisis resources for their region). If the situation echoes something specific, skim the relevant recent file(s) in `schema-log/` (e.g. `grep` the folder for the schema or the theme). Don't make the user re-explain what's already on record. If there's no `CLAUDE.md` yet, this is a first session: proceed normally and create it at the close (see below).

## Filename and location

- Path: `schema-log/YYYY-MM-DD-slug.md`, inside the working directory. Create the `schema-log/` folder if it doesn't exist.
- Slug: 2-4 kebab-case words naming the situation or theme, not the technique (`pushed-partner-away`, `boss-praise-shame`, `cant-say-no-mum`, `went-numb-argument`).
- Compute the date from the system clock with `python3` / `date` (in the user's local timezone if it's recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: schema-log
date: YYYY-MM-DD
schemas: [<schema names, kebab-case; canonical tags from core-model.md>]
domain: <disconnection-rejection | impaired-autonomy | impaired-limits | other-directedness | overvigilance-inhibition | other>
modes: [<modes active, kebab-case; canonical tags from core-model.md>]
coping-style: <surrender | avoidance | overcompensation; omit if not clear>
techniques: [<technique names used, kebab-case>]
intensity-before: <0-100 or 0-10>
intensity-after: <0-100 or 0-10>
tags: [schema-therapy, <schema tags>, <mode tags>, <technique tags>]
---

## Trigger

<The concrete situation, 1-2 sentences. Where, when, what set it off.>

## Schema & mode

<Which schema(s) fired and which mode(s) they flipped into, with the mode sequence if there was one (e.g. "trigger -> Vulnerable Child -> Detached Protector"). Belief in the core schema before -> after if rated.>

## Hot belief

<The core schema belief we worked, verbatim in their words (e.g. "I'm too much and people leave"), with belief % before -> after.>

## What we did

<Technique(s) run, in one or two lines. The key moment that moved it, or the one that didn't. For experiential work note whether it stayed stable and was debriefed.>

## Shift

<What changed: the new line to the Vulnerable Child, the Healthy-Adult response, the re-rating, or honestly "didn't move". Plain, not inflated.>

## Follow-up

<Any homework that fell out: a flashcard to carry, a schema-diary entry, a behavioral pattern-break to attempt, a letter to finish. Or "none". One concrete thing, not a program.>
```

Keep the whole entry under ~200 words. Past that it's a reflection, not a log, file a `reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it from `Follow-up`.

Never invent schemas, modes, feelings, or ratings the user didn't give. Missing data is `_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the structured tags (`schemas`, `domain`, `modes`, `coping-style`, `techniques`, `tags`) in canonical English kebab-case so the folder stays greppable, use the glossary mechanism in `core-model.md` to map the named schema / mode back to its English tag. But quote the **hot belief and the user's own words verbatim in the original language**, don't translate them; the fidelity of their actual phrasing is the data.

## Tagging for pattern-spotting

`tags:` and the `schemas` / `modes` / `domain` frontmatter are what make the folder greppable. Always include `schema-therapy` and the schema(s). Add the modes, coping style, and techniques used so a later review can answer "which schema fires most", "what trips the Detached Protector", "does imagery or chair work move the belief more", "which trigger keeps replaying". Don't pad with vague tags.

## Cross-linking

- If the session poked a recurring theme, the home for that is the `CLAUDE.md` "Schema profile" / "Mode map" / "Recurring patterns" sections, add or update it there rather than maintaining a separate web of links. To point at a specific past session, use a plain relative markdown link: `[pushed-partner-away](schema-log/2026-06-21-pushed-partner-away.md)`.
- If the homework is a **behavioral pattern-break** or a **letter**, record its result/whereabouts in this entry's `Follow-up`.
- If it grew into a real insight, file a `reflections/` note and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short, distilled context you read at the start and refresh at the close. The session logs are the raw data; `CLAUDE.md` is the summary — most importantly the user's **schema profile** and **mode map** — that makes the next session start warm instead of cold.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from any global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this one. Don't read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create it at the end of the session — always, every first session, no judgment call about whether anything was "durable enough." Fill only what you actually learned (leave the rest as placeholders, never invent). The "only if something durable changed" rule below governs *updates to an existing file*, not whether to create it. Use this template:

```markdown
# Schema self-help — working notes

Persistent memory for the schema-self-help skill. Read this at the start of every
session; update it at the end per the rules at the bottom. Keep it short and true.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Familiarity with Schema Therapy: _(not noted)_
- In therapy? therapist or trusted person to reach in a crisis: _(not noted)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once
known. See references/safety.md for how to source and verify these.)_

## Schema profile
_(The user's predominant Early Maladaptive Schemas and their domain, added as they
become clear across sessions. One line each, e.g. "Abandonment/Instability
(disconnection) — fires when a partner pulls back." Canonical names per
references/core-model.md.)_

## Mode map
_(The user's key modes, their triggers, and typical sequence — the running case
conceptualization. e.g. "Trigger: criticism -> Vulnerable Child (shame) ->
Detached Protector (goes cold). Punitive Parent loud. Healthy Adult: emerging,
strongest around work." Update as the map sharpens.)_

## Recurring patterns
_(Triggers, relationship dynamics, and themes that have shown up across more than
one session. Add an item once it recurs; keep each to a line.)_

## Open threads / homework
_(Flashcards carried, schema-diary kept, behavioral pattern-breaks attempted,
letters drafted, Healthy-Adult responses being practiced — and whether done.
Remove items once resolved.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that,
  update the sections if (and only if) something durable changed; otherwise leave
  it. Day-to-day detail stays in schema-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable things go here: a fact about the user changed, a schema or mode confirmed itself across sessions, the mode map sharpened, homework was set, or homework was resolved.
- **Edit in place; don't append endlessly.** Promote a schema into "Schema profile" once it recurs; sharpen the "Mode map" as the sequence clarifies; delete a thread from "Open threads" once it's done. The file should not grow without bound, if a section gets long, distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user", "Schema profile", "Mode map", and "Crisis resources" naturally from what the session reveals; don't open with a questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if `CLAUDE.md` and the logs disagree, the logs win, reconcile during a review.
- **Reconcile on review.** During a weekly / monthly look-back, re-grep `schema-log/` and update "Schema profile", "Mode map", "Recurring patterns", and "Open threads" so the summary still matches the data.

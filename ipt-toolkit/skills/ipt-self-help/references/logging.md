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

- `ipt-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled context, including the cast of important relationships). See the last two sections.

The user will not tell you where to write or what to do with `CLAUDE.md`. Run it yourself: read the context at the start, log at the end, keep `CLAUDE.md` current. Create `ipt-log/` and `CLAUDE.md` the first time they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This whole persistence layer assumes a writable working directory. If there isn't one — the skill is used in plain chat, no folder was picked, or writes simply fail — treat all of it as a no-op and move on: skip the start-of-session read, skip the `ipt-log/` write, skip `CLAUDE.md`. Do not stall, retry in a loop, or pressure the user to pick a folder. The session runs fully without persistence; only the cross-session memory is lost. Note it once in passing if relevant, then continue normally.

## Why log

IPT works through relationships, so the log's job is to build, across sessions, a picture of the user's interpersonal world: who the important people are, which relationship keeps generating distress, which problem area keeps recurring, what actually moves the mood. One entry is a session; the folder in aggregate is the interpersonal map. The log is **data, not a diary** — keep it structured and short.

## When to log

Log whenever a real walk-through happened — a technique was run, even partially, even if nothing shifted. Don't log a one-line exchange that never became a session. "Reconstructed the argument, couldn't see her side yet, anger unchanged" is a valid and useful entry; a manufactured win is not.

## Session start: read before you work

At the start of a session, before routing, read the working directory's `CLAUDE.md` if it exists — that's the fast context (what to call the user, their language, the cast of key relationships, the recurring problem area, any open next-move, crisis resources for their region). If the situation echoes something specific, skim the relevant recent file(s) in `ipt-log/` (e.g. `grep` the folder for the person's name or the theme). Don't make the user re-explain who their sister is or what last week's argument was about. If there's no `CLAUDE.md` yet, this is a first session: proceed normally and create it at the close (see below).

## Filename and location

- Path: `ipt-log/YYYY-MM-DD-slug.md`, inside the working directory. Create the `ipt-log/` folder if it doesn't exist.
- Slug: 2-4 kebab-case words naming the relationship or situation, not the technique (`dad-grief`, `wife-circular-arguments`, `since-the-baby`, `no-one-to-call`, `mum-the-move`).
- Compute the date from the system clock with `python3` / `date` (in the user's local timezone if it's recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: ipt-log
date: YYYY-MM-DD
problem: <one of: grief | role-dispute | role-transition | interpersonal-deficits | other>
person: <who the focal relationship is with, in the user's words: "wife", "dad (deceased)", "boss", "_(none specific)_">
dispute-stage: <renegotiation | impasse | dissolution; only for role disputes, omit otherwise>
techniques: [<IPT moves used, kebab-case: communication-analysis, encouragement-of-affect, decision-analysis, role-play, perspective-taking, reconstructing-relationship, balanced-inventory, ...>]
mood-before: <0-100 or 0-10>
mood-after: <0-100 or 0-10>
tags: [ipt, <problem>, <technique tags>, <person tag>]
---

## Interpersonal focus

<The relationship and the concrete situation, 1-2 sentences. Who, and what happened — the death, the argument, the change, the isolation. Note the temporal link to the mood if it was established.>

## Feeling

<The emotion(s) named, with intensity before -> after. e.g. "Grief 85 -> 65, plus guilt 70 about the things unsaid." Capture the full range if grief/dispute surfaced mixed affect (anger, relief, guilt).>

## What we did

<Technique(s) run, in one or two lines. The key moment that moved it — the line of the conversation that mattered, the feeling finally named, the option that clarified — or the one that didn't.>

## Shift

<What changed: the pattern they saw, the feeling that moved, the conversation rehearsed, the option they leaned toward (their decision, not yours). Plain, not inflated. Or honestly "didn't move".>

## Next move

<Any next step that fell out: a conversation to have, an approach to try, a person to reach out to, a feeling to sit with, a social step. Or "none". One concrete thing they chose — never a decision you made for them.>
```

Keep the whole entry under ~200 words. Past that it's a reflection, not a log — file a `reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it from `Next move`.

Never invent feelings, people, events, or ratings the user didn't give. Missing data is `_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the structured tags (`problem`, `techniques`, `tags`) in canonical English kebab-case so the folder stays greppable. But quote the **user's own words and the reconstructed dialogue verbatim in the original language** — don't translate them; the fidelity of how they phrased what someone said is the data.

## Tagging for pattern-spotting

`tags:` and the `problem` / `person` frontmatter are what make the folder greppable. Always include `ipt` and the `problem`. Add the focal `person`, the techniques used, and the dispute stage where relevant, so a later review can answer "how often does the dispute with my mother come up", "is it always a role transition", "does the same person keep appearing across different problems". Don't pad with vague tags.

## Cross-linking

- If the session poked a recurring relationship or theme, the home for that is the `CLAUDE.md` "Recurring patterns" and "Key relationships" sections — add or update it there rather than maintaining a separate web of links. To point at a specific past session, use a plain relative markdown link: `[dad-grief](ipt-log/2026-06-21-dad-grief.md)`.
- If a planned conversation or social step happened, record how it went in the next session's `Interpersonal focus` and `Shift`; there is no separate tracker.
- If it grew into a real insight, file a `reflections/` note and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short, distilled context you read at the start and refresh at the close. The session logs are the raw data; `CLAUDE.md` is the summary that makes the next session start warm instead of cold — and for IPT, that means knowing who the people are.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from any global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this one. Don't read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create it at the end of the session — always, every first session, no judgment call about whether anything was "durable enough." Fill only what you actually learned (leave the rest as placeholders, never invent). The "only if something durable changed" rule below governs *updates to an existing file*, not whether to create it. Use this template:

```markdown
# IPT self-help — working notes

Persistent memory for the ipt-self-help skill. Read this at the start of every
session; update it at the end per the rules at the bottom. Keep it short and true.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Therapist or trusted person to reach in a crisis: _(not noted)_

## Key relationships
_(The cast: the important people in the user's life as they come up — partner,
parents, children, friends, boss — with a word on the relationship. This is what
lets the next session start without re-introductions. Add people as they appear;
note who is living vs deceased.)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once
known. See references/safety.md for how to source and verify these.)_

## Recurring patterns
_(The problem area(s) and relationships that keep generating distress, and any
recurring interpersonal pattern (e.g. "ends up not saying what she wants, then
feels unseen"). Add an item once it recurs; keep each to a line.)_

## Open threads / next moves
_(Conversations to have, approaches to try, people to reach out to, social steps
set as a next move, and whether they happened. Remove items once resolved.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that,
  update the sections if (and only if) something durable changed; otherwise leave
  it. Day-to-day detail stays in ipt-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable things go here: a fact about the user changed, a new important person entered the picture, a pattern recurred (a problem area or relationship showing up a second or third time), a next move was set, or a next move was resolved.
- **Edit in place; don't append endlessly.** Promote a theme into "Recurring patterns" when it recurs; delete a thread from "Open threads" once it's done; update a relationship's line as it changes (a dispute resolved, someone died). The file should not grow without bound — if a section gets long, distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user", "Key relationships", and "Crisis resources" naturally from what the session reveals; don't open with a questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if `CLAUDE.md` and the logs disagree, the logs win — reconcile during a review.
- **Reconcile on review.** During a weekly / monthly look-back, re-grep `ipt-log/` and update "Recurring patterns", "Key relationships", and "Open threads" so the summary still matches the data.

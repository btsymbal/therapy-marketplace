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

- `sfbt-log/` — one markdown file per session (the data).
- `CLAUDE.md` — a short, living memory file at the folder root (the distilled context). See the last two sections.

The user will not tell you where to write or what to do with `CLAUDE.md`. Run it yourself: read the context at the start, log at the end, keep `CLAUDE.md` current. Create `sfbt-log/` and `CLAUDE.md` the first time they're needed; never ask the user to set anything up.

**When there's no filesystem at all.** This whole persistence layer assumes a writable working directory. If there isn't one — the skill is used in plain chat, no folder was picked, or writes simply fail — treat all of it as a no-op and move on: skip the start-of-session read, skip the `sfbt-log/` write, skip `CLAUDE.md`. Do not stall, retry in a loop, or pressure the user to pick a folder. The session runs fully without persistence; only the cross-session memory is lost. Note it once in passing if relevant, then continue normally.

## Why log

SFBT is built around progress over time — "What's better since last time?" only works if last time is on record. Every walk-through gets filed so that across entries the useful stuff surfaces: the client's goal and whether it's holding, which exceptions keep recurring (deliberate exceptions are the foundation to build on), where they are on the scale and which direction it's moving, what tasks were set and what came of them. One entry is a session; the folder in aggregate is the progress map. The log is **data, not a diary** — keep it structured and short.

## When to log

Log whenever a real walk-through happened — a technique was run, even partially, even if the scale didn't move. Don't log a one-line exchange that never became a session. "Set an observe-for-exceptions task, no exception found yet" is a valid and useful entry; a manufactured win is not.

## Session start: read before you work

At the start of a session, before routing, read the working directory's `CLAUDE.md` if it exists — that's the fast context (what to call the user, their language, their stated goal / best hopes, last scale position, open tasks, crisis resources for their region). If they're a returning user, this is what lets you open with EARS ("What's better since last time?") instead of starting cold. If the situation echoes something specific, skim the relevant recent file(s) in `sfbt-log/` (e.g. `grep` the folder for the theme or the goal). Don't make the user re-explain what's already on record. If there's no `CLAUDE.md` yet, this is a first session: proceed normally and create it at the close (see below).

## Filename and location

- Path: `sfbt-log/YYYY-MM-DD-slug.md`, inside the working directory. Create the `sfbt-log/` folder if it doesn't exist.
- Slug: 2–4 kebab-case words naming the goal or situation, not the technique (`steadier-mornings`, `same-fight-partner`, `restart-job-search`, `first-week-sober`).
- Compute the date from the system clock with `python3` / `date` (in the user's local timezone if it's recorded in `CLAUDE.md`), never mentally.
- If the file exists, append `-2`, `-3`.

## The entry format

```markdown
---
type: sfbt-log
date: YYYY-MM-DD
session: <first | follow-up>
problem: <one of: depression | anxiety | couples | family-parenting | child-adolescent | addiction | trauma | grief | self-esteem | workplace-coaching | other>
best-hopes: <the goal, short, in their words>
techniques: [<technique names used, kebab-case: miracle-question, exceptions, scaling, coping, relationship-questions, ears, ...>]
exceptions: [<short tags for exceptions found; omit if none surfaced>]
scaling-before: <0-10 or _(not noted)_>
scaling-after: <0-10 or _(not noted)_>
task: <the between-session task set, short; or none>
tags: [sfbt, <problem>, <technique tags>]
---

## Best hopes

<The goal, in their words, positive and concrete: what they want to be doing/feeling instead. 1-2 sentences.>

## Preferred future

<Key details from the miracle question / preferred-future picture, in their words. Who would notice, the first small sign. Omit if not reached this session.>

## Exceptions found

<Times a piece of the goal was already happening. Mark each random or deliberate — deliberate ones are what to build on. Omit the section if none surfaced.>

## Scaling

<Where they put themselves and why (what makes it that and not lower), the scale being used (progress / confidence / hope / safety), and what a one-point step up would look like. Before -> after if it moved.>

## What we did

<Technique(s) run, in one or two lines. The question that opened something up, or the one that didn't.>

## End-of-session task

<The compliment-bridge-task message's task, concretely: observational / behavioral / experimental, and exactly what. Or "none set".>

## Follow-up

<What to pick up next time: re-scale, check whether the task happened (no-blame), the next small step. Or "none". One concrete thing.>
```

Keep the whole entry under ~200 words. Past that it's a reflection, not a log — file a `reflections/YYYY-MM-DD-slug.md` (same working directory) for the deeper thread and link it from `Follow-up`.

Never invent a goal, an exception, a scale number, or a task the user didn't give. Missing data is `_(not noted)_`, not a guess.

**Session language.** If the session ran in a language other than English, keep the structured tags (`problem`, `techniques`, `tags`) in canonical English kebab-case so the folder stays greppable. But quote the **best hopes, the miracle details, the exceptions, and the user's own words verbatim in the original language** — don't translate them; the fidelity of their actual phrasing is the data.

## Tagging for pattern-spotting

`tags:`, `problem`, `techniques`, `exceptions`, and the `scaling-before/after` frontmatter are what make the folder greppable. Always include `sfbt` and the `problem`. Add the techniques used and any exceptions found so a later review can answer "is the scale trending up", "which exceptions keep recurring (and are they deliberate?)", "did the tasks get done", "what moved the goal forward". Don't pad with vague tags.

## Cross-linking

- If sessions are accumulating toward the same goal, the home for that is the `CLAUDE.md` "Goal & progress" section — update it there rather than maintaining a separate web of links. To point at a specific past session, use a plain relative markdown link: `[same-fight-partner](sfbt-log/2026-06-21-same-fight-partner.md)`.
- If a **task** was set, record its outcome in the *next* session's `Follow-up` (no-blame: "what got in the way?" is data, not failure); there is no separate task log.
- If it grew into a real insight, file a `reflections/` note and link both ways.

## The CLAUDE.md memory file

`CLAUDE.md` at the working-directory root is the skill's persistent memory: the short, distilled context you read at the start and refresh at the close. The session logs are the raw data; `CLAUDE.md` is the summary that makes the next session start warm — and, crucially for SFBT, lets you open with "What's better?" because you know where they were.

This is the skill's own file — Claude creates, owns, and maintains it. It is distinct from any global `~/.claude/CLAUDE.md`; the name collision is not a reason to avoid authoring this one. Don't read it as a config or system file you shouldn't touch.

**Creation is unconditional.** If there is no `CLAUDE.md` in the working directory, create it at the end of the session — always, every first session, no judgment call about whether anything was "durable enough." Fill only what you actually learned (leave the rest as placeholders, never invent). The "only if something durable changed" rule below governs *updates to an existing file*, not whether to create it. Use this template:

```markdown
# SFBT self-help — working notes

Persistent memory for the sfbt-self-help skill. Read this at the start of every
session; update it at the end per the rules at the bottom. Keep it short and true.

## About the user
- What to call them: _(not noted)_
- Working language(s): _(not noted)_
- Location / timezone (for dates and crisis resources): _(not noted)_
- Familiarity with SFBT: _(not noted)_
- Therapist or trusted person to reach in a crisis: _(not noted)_

## Crisis resources (local)
_(Local emergency number + 1-2 verified crisis lines for the user's country, once
known. See references/safety.md for how to source and verify these.)_

## Goal & progress
_(Best hopes in their words, the scale being tracked, and the last position on it.
Update the position each session so "What's better since last time?" has an anchor.)_

## What works for them (exceptions & resources)
_(Deliberate exceptions, coping strengths, and supportive people that have surfaced.
These are what to build on. Add one once it's shown up.)_

## Open tasks
_(The between-session task currently set and whether it was done. Remove items once
resolved or superseded.)_

## How this file is maintained
- Create the file on the first session unconditionally (see above). After that,
  update the sections if (and only if) something durable changed; otherwise leave
  it. Day-to-day detail stays in sfbt-log/, not here.
- See "Keeping CLAUDE.md sustainable" in references/logging.md for the full rules.
```

## Keeping CLAUDE.md sustainable

The file only stays useful if it stays short and current. Rules:

- **It's an index, not a journal.** One-off detail belongs in the session log. Only durable things go here: a fact about the user changed, the goal changed, the scale position moved, a deliberate exception or resource recurred, a task was set, or a task was resolved.
- **Edit in place; don't append endlessly.** Keep "Goal & progress" to the current goal and latest position; keep "Open tasks" to what's actually live, deleting resolved ones. The file should not grow without bound — if a section gets long, distill it.
- **Capture facts as you learn them, don't interrogate.** Fill "About the user" and "Crisis resources (local)" naturally from what the session reveals; don't open with a questionnaire.
- **Never invent.** Unknown stays a placeholder. The logs are the source of truth; if `CLAUDE.md` and the logs disagree, the logs win — reconcile during a review.
- **Reconcile on review.** During a weekly / monthly look-back, re-grep `sfbt-log/` and update "Goal & progress" and "What works for them" so the summary still matches the data.

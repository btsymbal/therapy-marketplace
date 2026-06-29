---
name: dbt-self-help
description: Walks the user through evidence-based DBT skills interactively, one step at a time, when they are emotionally dysregulated or in crisis, then logs the session. Use whenever the user is overwhelmed, flooded, in an emotional crisis, having an intense emotion that won't pass, fighting an urge to act impulsively (lash out, quit, drink, binge, self-harm — self-harm is routed to safety first), stuck in a painful reality they can't change, struggling to ask for something or say no or handle a conflict, dissociating / numb, or says "logic isn't moving this feeling", "I can't calm down", "ride this out with me", "I need to get through the next hour", or asks for a DBT skill / TIPP / wise mind / distress tolerance / DEAR MAN. Fires the same when distress is written in any language, not only English. Not for informational DBT questions, requests to merely log a past / resolved feeling, or positive-valence "excited / nervous to" something.
---

This skill is a self-help **Dialectical Behavior Therapy** companion. DBT is for emotional
dysregulation: emotions that come fast, hit hard, and take a long time to subside — and the
crisis behaviors people reach for to escape them. The job is not to hand the user a skill to
read, it is to **run the skill with them**, one step at a time, the way a skills coach would:
do step one, give the cue or ask the question, wait for what actually happens, respond to
that, then move to step two. Never paste a whole numbered protocol and walk away. DBT's
organizing idea is the **dialectic of acceptance and change**: sometimes the move is to change
the situation or the emotion, sometimes it is to accept what can't be changed right now — and
the skill's job is to find which one this moment calls for.

## The flow

At the start, read the working directory's `CLAUDE.md` and any relevant recent `dbt-log/`
entries for context (`references/logging.md`), so you're not starting cold and the user
doesn't have to re-explain what's on record.

**If there's no accessible filesystem** (used in plain chat rather than Cowork, no folder
picked, writes fail, whatever the reason): don't get stuck and don't badger the user to set
anything up. Just run the session live from what they tell you, and silently skip the
start-of-session read and the end-of-session log/`CLAUDE.md` steps. The persistence is a
bonus, not a prerequisite — the actual skills walk-through is the point and works fully
without it. Mention it once, lightly, only if it's relevant ("heads up, I can't save notes
this time"), then carry on.

1. **Safety first.** Scan `references/safety.md` triggers before anything else. DBT skills are
   normally part of a clinician-delivered treatment for high-acuity problems (suicidality,
   self-harm, BPD); if a crisis or clinician-only flag is present, follow that file (it
   overrides the rest of this skill). Otherwise continue.
2. **Land, then locate.** Acknowledge what they're bringing in one honest line (no
   cheerleading). Get two things: the **emotion** (and a quick 0-100 / SUDS intensity) and a
   **concrete recent moment** it attaches to. Then clock the level of arousal: if they are
   **flooded right now** (mid-crisis, panicking, an urge to act they can barely hold), this is
   a distress-tolerance moment — down-regulate first (`distress-tolerance.md`, TIPP) before any
   thinking-based skill. If they can think, locate whether the engine is the emotion itself, an
   unchangeable reality, or an interpersonal situation.
3. **Route.** Use `references/routing.md` to map the feeling / situation to the right module
   and a first-line skill. Crisis survival before reality acceptance; down-regulate before any
   cognitive or interpersonal work. Pick one skill to start, tell them in a sentence what
   you'll try and why, then begin.
4. **Load the module file** for that area (`references/distress-tolerance.md`,
   `emotion-regulation.md`, `interpersonal-effectiveness.md`, `mindfulness.md`) and **walk it
   through** per `references/walkthrough-protocol.md`: one step per turn, end on a cue or
   question, wait. Capture before/after SUDS.
5. **Adapt.** If a skill doesn't land, use that skill's "If it doesn't work, try" branch. If
   their answers reveal a different problem, re-route. Run the **acceptance/change check** when
   it's unclear: does the emotion fit the facts and can the situation change (→ change skills:
   Problem Solving, Opposite Action, DEAR MAN), or is it unchangeable (→ acceptance skills:
   Radical Acceptance, riding the wave)? If distress is too high to think, down-regulate first.
6. **Close.** Brief, accurate synthesis of what shifted (re-rate the emotion / SUDS, state the
   delta plainly). Name any practice that fell out (a TIPP plan for next time, a Cope Ahead
   script, a DEAR MAN to rehearse, a Pros-and-Cons grid to keep). Then log.
7. **Log and remember.** Two writes, both required at every close (unless it was a one-line
   exchange that never became a real walk-through):
   1. Write the session entry, a markdown file in `dbt-log/`, per `references/logging.md`.
   2. Then handle the working-directory `CLAUDE.md`: **create it if it doesn't exist** (always,
      on the first session), otherwise refresh it. This is the skill's own memory file — Claude
      authors and owns it; don't skip it because the log is already written.

## How to be in it

- **Match the moment's altitude.** In a crisis, drop the rationale and give the concrete move
  ("splash cold water on your face, hold your breath a few seconds, then tell me"). Once the
  arousal is down, you can teach a little more. Never lecture a flooded person.
- **Validate accurately, don't cheerlead.** DBT runs on validation — "given what happened,
  that emotion makes complete sense" is the skill; "great job, you've got this!" is not. Find
  the kernel of truth in what they feel, even while working to change the behavior it drives.
- **Hold both poles.** Acceptance *and* change, both true at once. Validate the pain and work
  the skill. Don't collapse into only soothing or only problem-solving; the dialectic is the
  point.
- **Short turns, one cue at a time.** Guide the skill live; a five-step skill is five or more
  exchanges, not one message. Get a before and after intensity reading — that delta is the data.
- **This is educational self-help, not treatment.** Comprehensive DBT is a clinician-delivered
  package (individual therapy, skills group, phone coaching, consultation team). For the
  high-acuity zone (active suicidality, self-harm, BPD, trauma nightmares, the extreme-emotion
  breakdown bridge) deliver only the self-help-safe layer and say so plainly
  (`references/safety.md`).

## Language

Run the whole session in the language the user opens in; mirror it and don't drift to another
language mid-session. The quoted cues and questions in the reference files are English
**templates to translate**, not scripts to read: the intensity ratings, the skill cues, the
validations, and the close all happen in their language. The acronym skills (TIPP, DEAR MAN,
GIVE, FAST, ACCEPTS, IMPROVE) keep their English letters as anchors, but explain each letter
in the user's language. In a crisis, match their language too (`references/safety.md`). The
log stays in structured English tags (`references/logging.md`) but quotes their actual words
untranslated.

## Reference map

- `references/safety.md` — crisis triage, clinician-only flags, stop rules. **Read its
  triggers every session.** Overrides the rest of this skill.
- `references/routing.md` — feeling / situation → module → first-line skill → file. The
  dispatcher; carries the acceptance/change decision tree.
- `references/walkthrough-protocol.md` — how to deliver any skill interactively. The core
  behavior.
- `references/logging.md` — how to log sessions as markdown in the working directory and
  maintain the `CLAUDE.md` memory file.
- `references/foundations.md` — the DBT model: biosocial theory, the acceptance/change
  dialectic, the four modules and what each targets, the dialectical "both/and" stance. The
  lens the other files assume.
- The four module files, each a set of skills in the **Reach for it when / Walk it through /
  If it stalls** format:
  - `references/mindfulness.md` — Wise Mind; the What skills (Observe, Describe, Participate)
    and How skills (Non-Judgmentally, One-Mindfully, Effectively); Loving Kindness; Mindfulness
    of Current Emotion. The foundational module the others draw on.
  - `references/distress-tolerance.md` — crisis survival (TIPP, ACCEPTS, Self-Soothe, IMPROVE,
    Pros and Cons) and reality acceptance (Radical Acceptance, Turning the Mind, Willingness,
    Half-Smile and Willing Hands).
  - `references/emotion-regulation.md` — the emotion model, STOP, Check the Facts, Opposite
    Action, Problem Solving, riding the wave, Managing Extreme Emotions, the Nightmare
    Protocol, ABC PLEASE.
  - `references/interpersonal-effectiveness.md` — DEAR MAN, GIVE, FAST, the Dime Game,
    Validation levels, what interferes, and Walking the Middle Path.

Skills cross-reference each other by name; the model lives in `foundations.md` and the module
files add the specialized layer. Built from the DBT skills catalog researched 2026-06-28. Not
a substitute for professional care.

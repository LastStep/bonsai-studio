---
name: brief
description: "The daily brief (document kind brief): what it is for, how to write it so the person can act on it from a phone, every field, and the template to copy. Use it when asked to write the day's brief. Not a brief to an agent (that is base:message)."
---

# The daily brief

The template of the document kind `brief`, which this pack declares in `bonsai/pack.yaml` (its folder; it has no id
and no statuses). A brief is one page a day for the person who directs the project: the one thing that needs them,
and what they can play or look at. The studio, not this pack, puts it in front of them; Bonsai has no clock, so it
is written when the studio's schedule or the person asks for it, by the role the project gives that work (a game
project's producer, `game:producer`; otherwise the orchestrator).

**Where it goes:** one file a day, named after its date (`2026-10-16.md`), in the folder the project's `bonsai.yaml`
gives the kind `brief` under `documents` (`work/briefs` unless the project changed it; `bonsai status --json` lists
it). Copy the template below, fill every field as the table says, and keep only the pointer comment on the first
line: the docs stay here.

## How to write it

- **Lead with the one thing that needs the person.** If nothing does, say so on line one and say what to play or
  look at instead. They read it on a phone before the day starts: short, plain words, no code.
- **Only what is true.** Everything in it comes from the files: the run reports, the task files, `git log`,
  `.bonsai/STATE.md`, the ladder's results. A task is done when its records say so, never because an agent said it.
  Never report progress you inferred.
- **A decision and a thing to play.** The brief exists to produce those two. It is not a status feed: what ran
  overnight is one line each, pointing at its run report.
- **The inbox stays small.** Only plans waiting for approval, the person's decisions and blocked tasks belong in it.
  An open ask is already on the Desk (`studio:desk-asks`): name it, do not copy it.

| Field | Meaning | Allowed values | Example |
|---|---|---|---|
| `date` | The day the brief is for; the file is named after it | a date, `YYYY-MM-DD` | `2026-10-16` |
| `author` | The role that wrote it | a role's name as the project loads it | `game:producer` |

```markdown
---
date: 2026-10-16   # the daily brief; every field and how to fill it: skill studio:brief
author: game:producer
---

# Brief, <weekday> <date>

**Needs you:** <the one thing, or "nothing: play the build if you have 20 minutes">

## Overnight

What ran and what landed, in plain words, one line each, pointing at its run report.

- <what is different now>: <the run report's id>

## Playable now

What the person can open and play or look at, and how: a link or a command. If nothing is new since they last
looked, say so rather than repeating yesterday's.

## Inbox

| What | Why it needs you | File |
|---|---|---|
| <plan, decision or blocked task> | <one line> | <the document's id> |

## Blocked

The task, the actual blocker in one plain sentence, and what would unblock it.

## Milestone

<milestone>: <n>/<m> done-when criteria met. <days> days to <date>. On track or at risk; if at risk, the fallback from
the plan.

## What it cost

Yesterday's and this month's spend, where the project records it.
```

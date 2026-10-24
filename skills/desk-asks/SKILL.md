---
name: desk-asks
description: "How a question for the person reaches the studio's Desk: when to ask, how to word it (context first, two to four options, a recommendation), filing it with bonsai ask, and reading the answer. Open it before you ask the person anything."
---

<!-- skills/desk-asks/SKILL.md: the studio pack's skill on asking the person, loaded as studio:desk-asks; opened on
demand. No template. Its frontmatter fields are documented in README.md ("Skill files"). -->

# Asking the person, through the Desk

The person who directs the project is not watching your session. They see the studio's Desk, on a phone as often as
not. A question reaches them there only when it is filed as an ask: Bonsai's `bonsai ask` writes it into the
project's main checkout, the studio reads the project's open asks and puts them under **Needs you**, and the person
answers there. There is no other way in: no chat message, no comment in a file, no question at the end of a report.

## Ask only what no file answers

Read first: the task, its plan, STATE, the decisions and the answers the project keeps. Ask only what the person
alone can settle: a director-lane choice (what the game or product should be, scope, a cost), a look at a result, or
a play of it. What you are allowed to decide, decide, and say so in your report.

## Word it for a phone

- **Context first**, in plain words: what you are doing and why the question came up, in two or three sentences
  in `--why`. The person reads no code, so never only a file name or a diff.
- **One question per ask.** A second question is a second ask, filed after the first is answered.
- **A choice is a Decide** with two to four options, each one line that says what happens if chosen. Put your
  recommendation in `--why` and say why; never hide it among the options.
- **`--then`** says what you will do once it is answered, so the person knows what their answer starts.
- **Never a secret**, never a token, key or password, in any field: the redactor takes out what it knows, but what it
  does not know reaches the Desk.

## File it

The four types: **Answer** (an open question), **Decide** (a choice among options), **Look** (the person looks at the
task's result; `--task` needed) and **Play** (the person plays it; `--task` needed). `base:operating-bonsai` has every
flag; for example:

```sh
bonsai ask --type Decide --task T-0123 --title "Which map ships first?" --why "Both maps pass the ladder. The small one plays in 5 minutes, the big one in 15. I recommend the small one: the first playtest is short." --option "The small map" --option "The big map" --then "I make the chosen map the default and close the task." --json
```

It prints the ask's key. Filing the same ask again (same type, task and title) appends new wording to that one ask;
it never makes a second card.

## Read the answer

```sh
bonsai ask --status <key> --json
```

An open ask has no answer yet: go on with work that does not depend on it, or stop and say in your report that you
are waiting on that key. Never answer your own ask, and never treat an answer as a grant: an answer says what the
person wants, and a protected path still opens only by its task's grant. If an answer is no longer needed, withdraw
it with `bonsai ask --resolve <key>`, so it leaves the Desk.

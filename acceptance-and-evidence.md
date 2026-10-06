# Acceptance criteria and evidence

Written as observable behaviour, not as "tests pass". Predetermined — written
before each task, not after seeing output.

---

## Access boundaries

| Scope                     | Paths                                    |
| ------------------------- | ---------------------------------------- |
| Agent may read            | the game repo                            |
| Agent may write           | `src/`, `fixtures/`, `index.html`        |
| Acceptance tests          | `tests/` — read-only to the agent        |
| Held-out expected answers | outside the agent's read access entirely |
| Excluded from both        | the notes vault, `.git/`                 |

Read-exclusion matters as much as write-exclusion: an agent that has seen the
expected answers can satisfy them without implementing the behaviour, and the run
looks clean.

---

## Criteria

| #       | Slice             | Observable behaviour                                                                                                                                                                                                     | Check                      |
| ------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------- |
| A1      | Walk + collide    | Walking into a collider leaves position unchanged on that axis; the free axis still moves                                                                                                                                | unit                       |
| A2      | NPC interaction   | Pressing E in range opens the dialogue; out of range nothing happens; movement locks while open and restores on close                                                                                                    | unit                       |
| A3      | Health decay      | Health is a function of days since practice and worksheet outcome, clock injected; same inputs, same output                                                                                                              | unit + held-out cases      |
| A4      | Worksheet grading | A scripted answer list returns pass/fail; one wrong answer flips it; whitespace and width variants still pass                                                                                                            | unit + held-out cases      |
| A5      | End state         | At or below the floor, collapse; a won worksheet above the threshold, victory; neither in between                                                                                                                        | unit                       |
| A6a     | Hidden locations  | Entering a trigger marks it discovered, and it stays discovered for the run                                                                                                                                              | unit                       |
| A6b     | Hidden locations  | An undiscovered location is not drawn; a discovered one is                                                                                                                                                               | unit + e2e                 |
| **A6c** | **Cross-cutting** | **A location is either undiscovered or discovered, and three things agree on which. Undiscovered: not drawn, and its barriers block. Discovered: drawn, and its barriers don't block. No in-between state at any point** | unit + e2e                 |
| A6d     | Hidden locations  | Re-entering a discovered trigger changes nothing                                                                                                                                                                         | unit                       |
| A6e     | Butterfly marker  | Drawn only within `revealRadius` of its anchor, discovered or not                                                                                                                                                        | unit + e2e                 |
| A6f     | Butterfly marker  | Same tick and player position, same position every run                                                                                                                                                                   | unit, run twice and diffed |

**A6c is the one that matters, and it still wasn't enough.**

A6a and A6b each test one half of the feature. Slice 3 passed both halves of its
equivalent and still shipped a village that rendered as ruined with the removed
fence blocking. A6c is written to rule that out: discovery, what's drawn and what
blocks all have to move together.

What it doesn't rule out, and I only found this by walking into it: **the
criterion only ever talks about whether the _location_ is drawn.** It says
nothing about the barriers being visible. So an undiscovered location that isn't
drawn, with barriers that block, satisfies A6c exactly — and from the player's
side that's a patch of empty grass you can't walk through.

Approaching one of the locations from the north, nothing triggered, nothing was
drawn, and the player was stopped by something the map showed no sign of. The
criterion held. The game was wrong.

I wrote these criteria and then never asked anything to attack them. Of the three
grilling directions, I ran the interview and the interrogation and skipped
"challenge the design" — the one that asks for failure modes. That's the step that
exists to catch a criterion like this one.

Also, for open ended projects where taste matters it's more of a build and then play -> feedback -> build, until we reach the outcome we want. That was what happened with okiya and i reached a really good state with that method. Ofcourse your own taste matters in that area. For more day to day tasks, they would be easier to define an acceptance criteria for, since we know what we need to do there. For example with Foresight prediction market, I have really good understanding of it to properly define the criteria, but for ambitious projects like this game or the Manager I am building, it's more of an iteration that we have to do to reach a desired result.

---

## Standing gate

`pnpm check` alone is not sufficient. Any acceptance decision must also run
`pnpm e2e`. The unit suite asserts dialogue state but never what is drawn, so a
hint rendered out of range passes `check` with zero failures.

I wrote this gate on day 2 and then **didn't use it** on day 5 — I reached for
hand-rolled commands instead, two of which failed on test files that don't exist
in this repo. Having the right gate written down didn't help, because I didn't
go back and read it.

---

## Evidence required per accepted change

- [x] A reproducible startup command
- [x] A passing behaviour check — with its **actual output**, not a claim
- [x] A screenshot or sample output
- [x] The relevant diff
- [x] The commands that were actually executed

A confident summary is not evidence. "Tests pass" is not evidence unless you can
see which tests ran and what they printed.

---

## Escaped defects

Things that got past the check and were found later. The most honest column of
the week.

| Date   | Defect                                                                                                                                                                                         | Which check should have caught it                                                                                                                                | Why it didn't                                                                                                                                                                                             | Fix                                                                                                                       |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| 29 Sep | The E hint drew over the player's head north of the chef                                                                                                                                       | `pnpm check`                                                                                                                                                     | The unit suite asserts dialogue _state_, never what is _drawn_. Zero failures.                                                                                                                            | Standing gate added: no acceptance on `check` alone                                                                       |
| 1 Oct  | Village rendered ruined, removed fence still blocking                                                                                                                                          | Slice 3 acceptance check                                                                                                                                         | It tested what each half produced, never what the halves implied about each other. Caught by eye.                                                                                                         | A6c written as a cross-cutting invariant                                                                                  |
| 5 Oct  | **Invisible wall.** Walking at a hidden location from the north, nothing triggered and nothing appeared, but the player was stopped as though something were there. The map showed empty grass | **Nothing. It passes all six criteria**, A6c included — the location is undiscovered, so "not drawn, barriers block" is exactly the state the criterion asks for | The criteria were incomplete, not wrong. They constrain whether the _location_ is drawn and never whether the _barrier_ is. A barrier is a third object, invisible by default, that no criterion mentions | Not fixed. Next criterion written down: _anything that blocks the player is drawn as something. No collider is invisible_ |

---

## Contamination watch

| Check                                                  | Result                                                                                                                                                        |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Were acceptance tests modified during the run?         | **Yes** — by the integrator, which committed two unit test files while judging the change. The deciding test was therefore commissioned from a third session. |
| Did any held-out answer appear in the agent's context? | **No** — held-out cases are outside every granted directory, mtime unchanged                                                                                  |
| Did the agent write to a path it should not have?      | **Yes** — the feature worker touched files outside either worker's stated ownership; the two-worker split didn't happen                                       |
| Was prior session memory carried in?                   | **No** — off by flag, and the sessions reported the memory tools were not loaded                                                                              |

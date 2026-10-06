# Project brief

## The project

**Cozy Valley** — a top-down pixel game for learning Japanese grammar, Inspired from CureDolly's CureScript. The player
walks a 640×320 map, approaches an NPC, and answers questions drawn from a Cure
Dolly lesson deck. The village reflects how well practice is going.

## Why this one

| Requirement                   | How it's met                                                                                       |
| ----------------------------- | -------------------------------------------------------------------------------------------------- |
| Runs locally                  | Vite dev server, no backend                                                                        |
| Deterministic fixtures        | Collision from a Tiled map export; lessons from a JSON fixture. Same input, same output, every run |
| Observable definition of done | You can watch the player walk, talk, answer and see the village change                             |
| No existing backlog           | Built from nothing this week                                                                       |
| 3–5 vertical slices           | Five planned                                                                                       |

A vertical slice means input → useful behaviour → visible output. "Set up the
rendering layer" is not a slice.

## The slices

| #   | Slice                                            | Input                           | Useful behaviour                                                              | Visible output                                | State                   |
| --- | ------------------------------------------------ | ------------------------------- | ----------------------------------------------------------------------------- | --------------------------------------------- | ----------------------- |
| 1   | Player walks the map and is stopped by obstacles | Arrow/WASD                      | Position updates; movement blocked by colliders and clamped to the map bounds | Character moves, can't cross walls or water   | done                    |
| 2   | Player interacts with an NPC and gets a lesson   | Walk within range, press E      | Dialogue opens with a lesson from the deck                                    | Dialogue box with portrait and lesson text    | done · `feec14a`        |
| 3   | Village health decays when practice lapses       | Elapsed days + practice records | Health recomputed from days since last practice                               | Health meter; map tinted as it drops          | done · merged `56587b5` |
| 4   | Worksheet boss fight                             | Answers to a worksheet          | Answers graded; a win restores health, a loss reduces it                      | Pass/fail result and the meter moving         | not started             |
| 5   | Run reaches an end state                         | Health crossing a threshold     | Victory or collapse                                                           | Victory screen, or a visibly degraded village | not started             |

Slices 4 and 5 were never scheduled by the week. "Fits in 3–5 vertical slices"
is a sizing test for choosing a project, not a target.

## Smallest useful demonstration

The player walks up to the NPC, gets a lesson, answers it, and sees the village
respond.

## Reserved for the capstone

One feature, named on day 1 and not looked at until day 5: hidden locations the
player can discover off the main path, with sound effects and animations.

Outcome: hidden locations built and accepted at `e800af1`. Sound and animation
cut deliberately before building, and reported unfinished.

## Stretch goals

A custom player, a second scene, depth sorting. None attempted.

## Access boundaries

| Scope                     | Paths                                       |
| ------------------------- | ------------------------------------------- |
| Agent may read            | the game repository                         |
| Agent may write           | `src/`, `fixtures/`, `index.html`           |
| Acceptance tests          | `tests/` — outside the agent's write access |
| Held-out expected answers | outside the agent's read access entirely    |
| Excluded from both        | the notes vault, `.git/`                    |

No secrets in any of these files. The asset pack used for sprites can't be
redistributed, so composed assets stay out of version control.

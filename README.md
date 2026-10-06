# AI-driven development — one week

Five days of structured practice building a small game with coding agents, with
notes on what the agents actually did and what they only claimed.

The project is **Cozy Valley**: a top-down pixel game where you walk a map, talk
to an NPC and answer Japanese grammar questions from a lesson deck. It was chosen
because it runs locally, uses deterministic fixtures, has an observable
definition of done, and had no existing backlog.

Claude artifact of the game: https://claude.ai/artifact/WUBi1CuhzMswfnS8MekVbM

## The four artifacts the program asks for

| File                         |                                                               |
| ---------------------------- | ------------------------------------------------------------- |
| `project-brief.md`           | One page: the project, the slices, the access boundaries      |
| `acceptance-and-evidence.md` | Acceptance criteria, what counts as evidence, escaped defects |
| `run-and-cost-log.md`        | Nine runs in the program's template, plus spend and quota     |
| `next-steps.md`              | What's unfinished, what's parked, what I'm changing           |

## The rest

| File                       |                                                           |
| -------------------------- | --------------------------------------------------------- |
| `REPORT.md`                | The write-up                                              |
| `OPERATING-PLAN.md`        | How I work from here                                      |
| `rediscovery-note.md`      | The idea I'd dropped, and what I tested                   |
| `LOG.md`                   | The running log, written as I went and trimmed afterwards |
| `evidence/day-1` … `day-5` | What I did each day, what happened, what I learned        |

## Where to start

`LOG.md` for how the week actually went. `evidence/` for it a day at a time with
the conclusions separated out. `run-and-cost-log.md` for the numbers.

## The code

The game is in a separate repository, `cozy-valley`, kept private because it
includes a purchased art pack that can't be redistributed. Every SHA cited in
these notes refers to it.
Ping me for access to the repo.

Two branches there are worth keeping: `run02-attempt-b` and `run03-codex` hold the
day-1 comparison — the same brief and the same starting commit, run two different
ways.

## On the numbers

Where a run passed or failed, the log gives the command and what it printed.
Where something was taken on trust instead of checked, it says so. Where a number
wasn't recorded at the time, it says that too.

# Day 5 — routines, a wrapper, and the capstone

**Oct 4–5.** A routine tested for idempotency, a CLI wrapper, and the held-back
feature from a fresh brief to an accepted change.

## What I did

- Built `scripts/triage.sh` — reads open issues, writes one prioritised report at
  a fixed path, overwriting it. Ran it seven times under varying conditions.
- Built `scripts/run-routine.sh` — wraps the CLI around a routine and verifies the
  routine's artifact from disk.
- Opened the held-back item, scoped it down twice, wrote six acceptance criteria,
  committed a contract, ran a graph, and had it reviewed on the other provider.

## The routine

The contract, written before the first run:

| Field             | Value                                                                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Trigger           | declared: a new issue file appears. **Not wired** — durable scheduling isn't set up, so it's invoked by hand, which the program permits as long as the missing feature is documented |
| Input             | the issues folder at a recorded commit                                                                                                                                               |
| Output            | a single file, outside the input folder, overwritten each run                                                                                                                        |
| Validation        | file exists · every open issue appears exactly once · each has a priority and a one-line reason · no issue invented                                                                  |
| Failure behaviour | write nothing and report the reason. Never a partial report — a half-written triage reads as a complete one                                                                          |

How each permission is actually held:

| Clause                     | Enforced by                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------- |
| No commits                 | **flag** — no Bash in the tool allowlist                                              |
| No memory                  | **flag** — no MCP servers loaded, no `mcp__*` in the allowlist, project-only settings |
| Write only the output path | prose                                                                                 |
| No edits to issue files    | prose                                                                                 |

Two of four. The routine writes exactly one file, so `Edit` can leave the
allowlist and move a third clause from prose to flag at no cost. The prose
clauses did hold under temptation — it found a factual error in an issue,
recommended fixing the file, and didn't touch it — but that's a result, not a
guarantee.

Four runs on frozen input:

| Run | Output path                  | Result                                       |
| --- | ---------------------------- | -------------------------------------------- |
| 1   | inside `issues/`             | baseline                                     |
| 2   | inside `issues/`             | **failed** — read run 1's output as an issue |
| 3   | vault root                   | still read the previous report               |
| 4   | vault root, previous deleted | **still not byte-identical**                 |

Stable across every run: the five issues, their order, their priorities, the link
targets. Rewritten every run: the header sentence, every reason, four of five link
labels, and the priority key — which changed from one line into a three-bullet
section.

My note at the time:

> Prose can be different, so we need to make sure structure at least is the same —
> and we do that by specifying the output format in the contract, so at least
> that's identical. Semi-idempotent.

**Then the failure mode that matters.** With the input folder renamed away, the
routine correctly wrote nothing and explained why — and exited 0. The previous
report sat there looking complete, passing all five content checks.

## The wrapper

| Requirement               | How                                                                             |
| ------------------------- | ------------------------------------------------------------------------------- |
| Task input                | `run-routine.sh <routine> [args]`                                               |
| Structured output         | a JSON schema over status, path, exit code, message                             |
| Outer timeout             | watchdog on its own process group, TERM then KILL                               |
| Parses the result         | validates the envelope and the output shape                                     |
| **Verifies the artifact** | from disk: exists, **newer than the run's start**, non-empty, valid, structured |

It failed a good run first, because the agent _described_ the artifact by a path
it had guessed. Then it failed a good run in the opposite direction, because the
agent honestly reported it couldn't observe an exit code.

Both fields it reported in the first demo were guesses, and it said so: it
resolved a path against the wrong root, and inferred an exit code of 0 from the
absence of an error. The schema forced them to be well-formed; it couldn't force
them to be true. Asking for a value the reporter can't observe gets you a guess
or a refusal, and neither is worth having.

Final shape: the agent's status is a warning and never sets the verdict. The
verdict is the artifact on disk. The routine prints a success sentinel as its last
line, so its absence is the failure signal.

**Proof:** input folder renamed away, run through the wrapper. The routine exited
0 and printed its sentinel. The agent reported success. The wrapper returned
`failed`, exit 1, `not written during this run (mtime predates start)`.

## The capstone

Two scope cuts, both recorded before any code:

1. Hidden locations is the slice; sound and animation are cut and reported
   unfinished.
2. The butterfly cut from a guide you follow to a marker that appears when you're
   near. That removed all its state, so watching it run became enough and reading
   its code stopped being necessary.

Six criteria written before the first prompt. One of them cross-cutting, because
of slice 3:

> **A6c** — a location is either undiscovered or discovered, and three things
> agree on which. Undiscovered: not drawn, barriers block. Discovered: drawn,
> barriers don't block.

Contract committed first at `bd127b9`, types and signatures only. `blocksMovement`
and `visible` both read the same set, so A6c holds by construction.

**The graph never ran as a graph.** I specified five nodes in prose and pasted the
description into a session rather than executing a script. The spec node produced
no file and the build ran anyway; one agent did both workers' jobs across both
ownership boundaries; the integrator committed to the tree it was judging.

**First review** found A6c unestablished — the renderer could cache its own input
and nothing would notice. A third session wrote the deciding test; both breakages
failed it, one of 243 each time.

**Second review, other provider:** fifteen mutations, ten caught, five survived
the full gate and were each confirmed behavioural with a probe. Three of the five
were in the _second_ hidden location — the fixture has two and every test uses
one. So it only caught one hidden location in tests and the other kept failing, until caught by codex as the verification agent.

**Then I walked into it from the north** and was stopped by a barrier drawn as
nothing. It passes all six criteria.

## The grilling direction I skipped

The program lists three directions. For the capstone I ran two.

| Direction | Ran |
| --- | --- |
| The agent interviews me | yes, before the contract |
| **The agent challenges the design** — fragile assumptions, the simpler alternative, **failure modes**, the cheapest experiment that would change the decision | **no** |
| I interrogate the agent | yes — the integrator, then the second provider |

The one I dropped is the one whose whole job is naming failure modes, and I
dropped it for time.

There was a second chance built in and it didn't fire either: the design-spec node
was told the criteria were fixed inputs and that if it thought one was wrong it
should file an objection and stop. It produced no file at all, so no objections
came back. That isn't evidence it had none — nothing ran.

I can't test now whether it would have caught the invisible wall. But the question
that direction asks — what are the failure modes of this design — against a
feature whose entire purpose is hiding things from the player, is the one most
likely to have surfaced "and how does the player know the barrier is there?"

## What I learned

**A routine must not be able to see its own output.** Moving the file out of the
input folder wasn't enough; the directory grant still covered it.

**Content checks can't tell a finished run from a failed one.** Only freshness
can. Existence, size, encoding and structure all passed on a stale file.

**A graph you describe but don't execute is a long prompt.** I'd been careful
about this all week with CLI flags — memory off because no servers loaded, no
commits because Bash wasn't in the allowlist — and then set the orchestration up
as paragraphs.

**Criteria get verified against whichever single instance came to mind.** One
location, one tick value, one entry direction. Every gap traced back to that. It didn't cover me going from the top of the hidden location and I would get blocked with the map showing walkable, and had to enter from a specific direction to unlock the location.

**The criteria themselves can be the hole.** The invisible wall isn't a bug in the
code — the code does what the criteria say. The only instrument that finds a hole
in your criteria is using the thing.

**Taste** still remains on the human, with the okiya game I was the one steering the design, the feel, and the playability of the game, alongside fable working as a design advisor. But here I didn't run it at that effort and was visible. So while it still passed the criteria, there was a huge difference in the feel of playing the game.

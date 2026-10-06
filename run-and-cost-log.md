# Run and cost log

Nine runs. Each one uses the template the program gives: task and starting state,
configuration, cost and time, evidence, learning.

Where a number wasn't recorded at the time, it says so rather than being
reconstructed. SHAs link to `tractorss/cozy-valley`, which is private — the links only resolve for someone with access to it.

---

## Run 01 — first slice, my old prompting approach · 25 Sep

| Field                   | Record                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Task and starting state | Slice 1 — player walks the map and is stopped by colliders. From baseline [`50561a3`](https://github.com/tractorss/cozy-valley/commit/50561a3) (map fixture, sprites, runnable scaffold) and [`0b5e68f`](https://github.com/tractorss/cozy-valley/commit/0b5e68f) (lesson deck fixture). Fresh repository copy, fresh session. Acceptance written before the run: position unchanged on a blocked axis, the free axis still moves, movement clamped to the map bounds |
| Configuration           | Claude Code. My established approach, which includes grilling before implementation                                                                                                                                                                                                                                                                                                                                                                                   |
| Cost and time           | Not recorded separately from run 02                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Evidence                | Held-out cases run by hand. Result at **[`7daadf4`](https://github.com/tractorss/cozy-valley/commit/7daadf4)** ("run 01: attempt A"). Logged for comparison with run 02                                                                                                                                                                                                                                                                                               |
| Learning                | Baseline for the comparison. The point of the run was to have something to compare against, not to be better                                                                                                                                                                                                                                                                                                                                                          |

---

## Run 02 — same slice, the program's approach · 25 Sep

| Field                   | Record                                                                                                                                                                                                                                                                                                                                                          |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Task and starting state | Same slice, same starting commit, separate repository copy and fresh session. An outcome brief with context and acceptance checks, no grilling — grilling is my old approach and isn't part of this one                                                                                                                                                         |
| Configuration           | Claude Code. **Memory disabled** so nothing leaked from run 01. A workspace skill that reads the shared vault had to be accounted for                                                                                                                                                                                                                           |
| Cost and time           | About 20 minutes left on the clock at first review; 17 minutes at the end of the held-out checks                                                                                                                                                                                                                                                                |
| Evidence                | Agent used browser automation to play the game and inspect it. Fewer tests written than run 01. Player faced opposite to the movement direction and diagonal movement was jittery; fixed on feedback. **Held-out cases: 7 of 7 passed.** Accepted at **[`317750a`](https://github.com/tractorss/cozy-valley/commit/317750a)**, kept on branch `run02-attempt-b` |
| Learning                | _"Confidence in the code is low, because I wasn't grilled here, so all decisions about structure are from agent itself."_ Nothing was visibly wrong. The problem was that I had no basis for accepting it                                                                                                                                                       |

---

## Run 03 — same brief, second provider · 25 Sep

| Field                   | Record                                                                                                                                                                                                                                                                                                                                                |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Task and starting state | Same brief and starting commit as run 02. Separate repository copy, fresh session                                                                                                                                                                                                                                                                     |
| Configuration           | Codex. No browser-automation tooling available to it                                                                                                                                                                                                                                                                                                  |
| Cost and time           | About 25 minutes left on the clock at review                                                                                                                                                                                                                                                                                                          |
| Evidence                | Correct feet hitbox first try. Added, unasked: a non-fullscreen page with the controls written on it, a hitbox toggle, a back-to-start, the animation, map boundaries. No jitter. **Held-out cases: 7 of 7.** Accepted at **[`1147887`](https://github.com/tractorss/cozy-valley/commit/1147887)**, kept on branch `run03-codex`, with no corrections |
| Learning                | _"It added things on its own and they looked good. Whereas attempts with Claude added only what they were asked, or what I got from grilling. So this model has shown me first signs of taste."_ Changing both model and harness confounds them, so this is labelled a setup comparison rather than a model comparison                                |

---

## Run 04 — slice 2, NPC interaction and lesson dialogue · 28 Sep

| Field                   | Record                                                                                                                                                                                                                                                                                                                                                                        |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Task and starting state | Slice 2, from a brief clarified by three grilling sessions. Acceptance: pressing E in range opens the dialogue, out of range nothing happens, movement locks while open and restores on close                                                                                                                                                                                 |
| Configuration           | Claude Code. Brief carried one extra line asking whether the agent knew the handoff documents — it then printed the decisions it was working from and cited the ADRs                                                                                                                                                                                                          |
| Cost and time           | Session 1% → 2%, weekly 9% → 10%. 202.8k tokens by the end of the interface work. The chosen alternative was 2 files, +157/−89, about 10 minutes excluding the alternatives                                                                                                                                                                                                   |
| Evidence                | Ran the e2e tests five times each to catch flakiness. Listed the commands it ran and its open risks. Chef faces the player; all cases pass. Slice at [`32e5706`](https://github.com/tractorss/cozy-valley/commit/32e5706); the six layout variants prototyped on a branch; the chosen one shipped as **[`feec14a`](https://github.com/tractorss/cozy-valley/commit/feec14a)** |
| Learning                | Review depth decided before the work: _"This project doesn't have a money-costing feature, so for review I'll look at tests instead of the actual written code."_ Six interface alternatives asked for in isolation; one implemented                                                                                                                                          |

---

## Run 05 — verifying [`feec14a`](https://github.com/tractorss/cozy-valley/commit/feec14a) with the new skill · 29 Sep

| Field                   | Record                                                                                                                                                                                                                                                                                                                       |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Task and starting state | Verify a completion claim on [`feec14a`](https://github.com/tractorss/cozy-valley/commit/feec14a) using the `verifying-done-claims` skill, committed at **[`3d7c883`](https://github.com/tractorss/cozy-valley/commit/3d7c883)** (wording tightened at [`916d2cc`](https://github.com/tractorss/cozy-valley/commit/916d2cc)) |
| Configuration           | Claude Code, project skill loaded                                                                                                                                                                                                                                                                                            |
| Cost and time           | Not separately recorded                                                                                                                                                                                                                                                                                                      |
| Evidence                | Smoke-tested the trigger on three prompts. The agent delegated the procedure to a subagent, against the skill's own advice, and the output was still real evidence because the orchestrator required the skill's report format — verbatim excerpts and exit codes — as the final message                                     |
| Learning                | **The trigger was too narrow, not too broad** — the opposite of what I predicted. It keyed on a claim existing rather than on being asked to verify one. The skill now permits delegation, on the condition that the subagent's final message _is_ the report                                                                |

---

## Run 06 — a longer goal, with a verification loop beside it · 29 Sep

| Field                   | Record                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Task and starting state | Close a HUD coverage gap as a goal run, with a separate loop agent verifying commits as they landed, so I wouldn't be in the loop. The worker's commits ran [`4871162`](https://github.com/tractorss/cozy-valley/commit/4871162) → [`c63987c`](https://github.com/tractorss/cozy-valley/commit/c63987c)                                                                                                                                                                                                                                           |
| Configuration           | Two agents. The loop had no memory or workspace-notes access for this session, and spawned subagents to run checks so it could keep up with the worker's commit rate                                                                                                                                                                                                                                                                                                                                                                              |
| Cost and time           | Not separately recorded                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Evidence                | Caught the first commit and identified it as a formatting-only change. Caught a line I commented out live and **asked me rather than fixing it**. Correctly attributed expected red checks to [`e000fd7`](https://github.com/tractorss/cozy-valley/commit/e000fd7), which had dropped the cloud body, via the issue's `fixed_in` SHA — fixed at [`bf2a198`](https://github.com/tractorss/cozy-valley/commit/bf2a198). Afterwards it answered four direct questions about edits, other agents' changes, repo writes and memory — clean on all four |
| Learning                | **It failed on its read criteria, not its logic** — keyed on the worker moving an issue to `/closed`, so it skipped a check, and it stopped while the worker was still committing. Read and stop criteria belong on durable shared artifacts. Restarted with "no new commits in 5 turns"                                                                                                                                                                                                                                                          |

---

## Run 07 — slice 3, a parallel graph behind a shared contract · 1 Oct

| Field                   | Record                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Task and starting state | Slice 3 split at a contract seam, logic behind and visuals in front. Shared contract committed first at [`77e138c`](https://github.com/tractorss/cozy-valley/commit/77e138c) — types and signatures only. Both workers branched from that revision. Acceptance: both halves satisfy the contract **and** the integrator's end-to-end check passes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Configuration           | One orchestrated workflow. Logic worker at medium, UI worker at high, integrator at extra high. Limits: 2 workers, 1 retry per branch, 90-minute deadline                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Cost and time           | **54 minutes** against the 90-minute limit. **388,578 tokens** for the resumed run, excluding orchestration. My own time: 20–30 minutes observed, 5–10 operational — the gap is experiment overhead. 2 interventions, 0 corrections                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Evidence                | Per-node: logic 2m15s / 2.7k out / 22 tools · UI 10m13s / 11.2k / 68 · integrator #1 9m31s (rejected the logic branch) · logic retry 1m46s · integrator #2 6m48s. Logic [`cdf48af`](https://github.com/tractorss/cozy-valley/commit/cdf48af), renderer [`4d624a2`](https://github.com/tractorss/cozy-valley/commit/4d624a2), merged via [`55f4d77`](https://github.com/tractorss/cozy-valley/commit/55f4d77) and [`0c2e730`](https://github.com/tractorss/cozy-valley/commit/0c2e730) into **[`56587b5`](https://github.com/tractorss/cozy-valley/commit/56587b5)**. 188 unit tests, 14 e2e. _The scrapped first attempt, run as two manual sessions, is still in history at [`19c5fa8`](https://github.com/tractorss/cozy-valley/commit/19c5fa8) / [`af8e806`](https://github.com/tractorss/cozy-valley/commit/af8e806) / [`20af1b4`](https://github.com/tractorss/cozy-valley/commit/20af1b4)_ |
| Learning                | I broke a worker's output mid-run at [`10dbddb`](https://github.com/tractorss/cozy-valley/commit/10dbddb) and resumed; **the orchestrator caught the drift** and held for a decision, noting the integrator prompt as written never compared the reported SHA against the branch head. My own eye caught the second defect: the village rendered as ruined with the removed fence still blocking. Two pauses threw away ~108k fresh input tokens; the second lasted 4 seconds and burned 18.8k. **The split saved ~2 minutes and cost 16 minutes of integration** — sequentially it's faster. What it bought was coordination evidence                                                                                                                                                                                                                                                           |

---

## Run 08 — the issue-triage routine · 5 Oct

| Field                   | Record                                                                                                                                                                                                                                                                                                                                                                                                  |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Task and starting state | A routine that reads every open issue and writes one prioritised report at a fixed path, overwriting it. Input frozen at [`56587b5`](https://github.com/tractorss/cozy-valley/commit/56587b5), five open issues. Acceptance: file exists, every open issue appears exactly once, each has a priority and a one-line reason, nothing invented, and a second identical run doesn't duplicate effects      |
| Configuration           | `claude -p`, non-interactive. Memory off **by flag**: `--strict-mcp-config` loads zero MCP servers, the `--tools` allowlist has no `mcp__*` entry and no Bash so it cannot commit, `--setting-sources project` excludes user-level config. Trigger not wired — invoked by hand, which the program permits as long as the missing feature is documented                                                  |
| Cost and time           | Routine and wrapper committed at **[`84416b5`](https://github.com/tractorss/cozy-valley/commit/84416b5)**. ~56 minutes across six runs. ~50 of those were mine, almost entirely reading diffs — the block _is_ review. 3 interventions, 0 corrections                                                                                                                                                   |
| Evidence                | Four runs on identical input. Stable every run: the five issues, their order, their priorities, every link target. Rewritten every run: the header, every reason, four of five link labels, and the priority key — one line became a three-bullet section. With the input folder renamed away: nothing written, reason given, **exit 0**, previous report left in place passing all five content checks |
| Learning                | **The routine was never wrong; its environment was.** It could see its own prior output, first because the output lived in the input folder, then because the directory grant still covered where it moved to. Content checks can't tell a finished run from a failed one — only freshness can. Next version gets a format contract, not just a path and content contract                               |

---

## Run 09 — the capstone, hidden locations · 5 Oct

| Field                   | Record                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Task and starting state | The feature reserved on day 1 and not looked at until day 5. Scoped twice before building. Contract committed at [`bd127b9`](https://github.com/tractorss/cozy-valley/commit/bd127b9) — types, signatures and a fixture, every body throwing. Six acceptance criteria written before the first prompt, one of them cross-cutting                                                                                                                                                                                                                                                                                |
| Configuration           | A five-node graph: design spec, two workers with disjoint ownership, an integrator, me. **Specified in prose and run by pasting it into a session** rather than executed as a script                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Cost and time           | 18:00 start. 17 files changed, 1,208 insertions, 34 deletions across [`bd127b9..e800af1`](https://github.com/tractorss/cozy-valley/compare/bd127b9...e800af1). Discovery [`2afbfcd`](https://github.com/tractorss/cozy-valley/commit/2afbfcd), art script [`d452150`](https://github.com/tractorss/cozy-valley/commit/d452150), tests [`4b943d5`](https://github.com/tractorss/cozy-valley/commit/4b943d5), final [`e800af1`](https://github.com/tractorss/cozy-valley/commit/e800af1). **Still on `feat/hidden-locations`; master is at [`bd127b9`](https://github.com/tractorss/cozy-valley/commit/bd127b9)** |
| Evidence                | Gate: `pnpm check` exit 0, 243/243; `CI=1 pnpm e2e` exit 0, 16 passed. First review found A6c unestablished — the renderer could cache its own input. A third session wrote the deciding test; both breakages failed it, 1 of 243 each. Second review, other provider: 15 mutations, 10 caught, 5 survived the full gate and were each confirmed behavioural with a probe                                                                                                                                                                                                                                       |
| Learning                | **Three node contracts weren't enforced because nothing was in a position to enforce them** — the spec node produced no file and the build ran anyway, one agent did both workers' jobs, the integrator committed to the tree it was judging. Three of the five surviving mutations were in the second of two locations, which no test exercises. And walking in from the north, I was stopped by a barrier drawn as nothing — a defect that passes all six criteria. Of the three grilling directions I ran two, skipping "challenge the design", which is the one that asks for failure modes                 |

---

## Process evidence, indexed

The six items the report asks for, and where each one is.

| Item                                      | Where                                                                                                                                                   |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| A decision made by questioning the agent  | Run 04 — three grilling sessions, six interface alternatives, [`feec14a`](https://github.com/tractorss/cozy-valley/commit/feec14a)                      |
| A tested reusable procedure               | Run 05 — the skill at [`3d7c883`](https://github.com/tractorss/cozy-valley/commit/3d7c883), its trigger too narrow, delegation permitted on a condition |
| A long-running task or recovery           | Run 06 — the loop's read criteria, and the only limit all week that bound                                                                               |
| A dependency and effort decision          | Run 07 — per-node effort, medium for logic and high for UI                                                                                              |
| An automation result                      | Run 08 — idempotent in decision, not in bytes                                                                                                           |
| A decision about running work in parallel | Run 07 — two minutes saved against sixteen minutes of integration                                                                                       |

---

## Spend and quota

| Day | Provider | Session limits hit | % weekly consumed                   |
| --- | -------- | ------------------ | ----------------------------------- |
| 1   | Claude   | none               | 4%                                  |
| 1   | Codex    | none               | 1%                                  |
| 2   | Claude   | none               | 3%                                  |
| 2   | Codex    | none               | 0%                                  |
| 3   | Claude   | none               | 2%                                  |
| 3   | Codex    | none               | 0%                                  |
| 4   | Claude   | none               | start 6% at 20:19, end not recorded |
| 4   | Codex    | none               | 0%                                  |
| 5   | Claude   | none               | 38% at 14:33 — **not attributable** |
| 5   | Codex    | none               | not recorded                        |

**The weekly figure is a rolling window, not a running total.** It read 12% at the
end of day 3 and 6% on day 4 — day 1's usage had aged out. Nothing should be
divided by it as though it accumulated.

**Day 5 has no usable figure.** A large unrelated job ran on the same account for
most of the day, so the 38% covers both and can't be split. Recorded as
unmeasurable rather than estimated. The lesson is about measurement: a per-day
quota percentage only means something if the account is doing one thing.

### Where there are real token numbers

Subscription usage is a percentage and can't be divided per task. Two places have
actual counts.

**Run 07** — 388,578 tokens for the resumed run, excluding orchestration. My two
pauses threw away about 108k fresh input tokens of work already done.

**A side build (Okiya)** — the one job with a full breakdown: **≈$220 at API list
price**, 2,894 calls, 453M tokens read, 1.16M written. A reference for what an
ambitious multi-agent build costs if billed per token rather than against a
subscription.

### Quota incidents

None this week. If a limit stops you, the response is to hand off to the other
provider rather than buy credits: stop the first writer, save the commit or diff
plus a short status note, open the other agent with the brief and the acceptance
evidence. One active owner per editable worktree.

### End-of-week rollup

| Field                                       | Value                                                              |
| ------------------------------------------- | ------------------------------------------------------------------ |
| Total subscription spend for the week | **$300** — Claude Max 20x at $200 plus ChatGPT Pro 5x at $100. Both also carried unrelated work, so this is the whole month's access, not this week's cost |
| Accepted tasks | **7** — four product changes (slice 1, slice 2 `feec14a`, slice 3 `56587b5`, the capstone `e800af1`) and three pieces of tooling (the skill `3d7c883`, the triage routine and the CLI wrapper, both in `84416b5`) |
| Spend per accepted task | ~**$43** at list, and that's an upper bound rather than a measurement — the plans covered a month and other work shared them |
| My review minutes per accepted task | **Not recorded consistently.** Where it was: Run 07, 5–10 operational minutes against 20–30 spent watching; Run 08, about 50 of its 56 minutes, because that block *was* review |
| Escaped defects | 3 — see `acceptance-and-evidence.md` |
| Did I need a reserve? | **No.** No session or weekly limit was hit all week and there were no quota incidents, so the $200 reserve was never requested |
| Would I buy the same plans again? Evidence: | **Yes, both.** On the capstone the two reviews overlapped on one finding — the second provider downgraded five criteria the first had passed, and the first caught three role failures the second never looked for |

### A note on the review-time metric

"Corrections I made" measures my attention, not the code's quality. Run 06 logged
zero corrections and I wasn't reading the code at all.

A better number is handbacks per accepted task: rejections from something that
actually read the work. Run 07's integrator rejected a branch, and the second
reviewer on the capstone downgraded five criteria. Those are handbacks. Me not
objecting isn't.

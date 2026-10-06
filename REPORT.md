# Training report

---

## 1. Project and accepted result

What got built:

Three slices: movement and collision; NPC interaction with lesson dialogue (feec14a); village health decay (56587b5). Plus the held back feature of hidden locations with discovery, unlock and a butterfly marker (bd127b9..e800af1, merged at 2e0269b).

Checked by: acceptance criteria written before each run, held-out cases I ran manually, a standing gate of pnpm check, pnpm e2e, mutation testing, and for the heldback case an integrator followed by a codex review that ran fifteen deliberate breakages.

Unfinished: slices 4 and 5 were never scheduled; sound and animation cut before building the held back case. one of six criteria, and three defects are known and unfixed (invisible wall, player drawn over props, parser rejecting valid geometry)

The accepted result isn't a "game" , it's more of what the project asked and needed to be worked on. A better example of the learnings would be the okiya game that we built on the fly, which was a real ambitous project and turned out great.

→ `project-brief.md` · `evidence/day-5-routines-and-capstone.md` · `next-steps.md`

---

## 2. One rediscovery

I stopped work on this project before because the game itself required substantial creative thinking and map design, with asset building and composing them. On top of that, it was previously being built with Godot and felt too big of tasks to take on alongside work.

What became possible: implementation stopped being the constraint. Three slices and a held back in a week, alongside a job.

What still needed my judgment: taste,: on Okiya I steered the design and feel with an advisor at high effort and it came out well; here I didn't run it that way, and it passed every criterion while feeling wrong to play. Also the invisible wall, which no automated check could have found. So the acceptance tests should be constructed with that in mind, for UI or things that require taste, I find it better to have the agent run another subagent to do a deep research in that field, for okiya the deep research was around game building and human physchology around games and it paid well, along with the fable as designer/advisor/critique

What I'll try next: The foreman app for now, which is a personal management system, since we will be transitioning to more of a manager, and If I get leisure time, maybe keep working on cozy valley and find a way to have agent handle the map generation too with actual playablity

→ `rediscovery-note.md`

---

## 3. Current setup

Providers : Claude (200$) , codex (100$)

Harnesses: Claude, Codex, Engram, Workspace-notes (Maintains an obsidian vault for human readable docs, adrs, specs, etc), Playwright mcp, cmux

Effort settings: Dependent on task, default to medium. Opus as default, Fable as advisor, Codex as reviewer.

This would change depending on more learnings and feedback I get from my own findings. No session or weekly limit was hit during the setup, but I do expect them to hit from here on, given the usage it took for okiya and foreman app.

→ `run-and-cost-log.md`, the spend and quota section

---

## 4. Process evidence

**Questioning the agent:** on day 2 I added one line to the brief asking if the agent knew about the handoff documents. It then printed the decisions it was working from and referenced the ADRs and engram, instead of starting from the brief alone. Run 04, `feec14a`.

**Tested skill:** `verifying-done-claims`, committed at `3d7c883`. I expected the trigger to be too broad. It was too narrow, it fired on "check if the work was done correctly" and not on "is the work complete?". Run 05.

**Goal and recovery:** Run 06. The loop was watching for the worker to move an issue into `/closed`, so when the worker didn't, it skipped the check, and it stopped while the worker was still committing. Read and stop criteria need to sit on something durable, like PRs, issues, commits and not a folder convention. Here it was used because it was faster to test.

**Effort decision:** Run 07. Logic at medium was correct first time, 2m15s and 22 tool calls. UI at high took 10m13s and 68 tool calls against real ambiguity. Integrator at high caught a real rejection.

**Automation result:** Run 08, `scripts/triage.sh`. Same decisions every run, never the same bytes. And with the input folder renamed away it wrote nothing, said why, and exited 0, leaving the old report sitting there looking complete. So the real change test should be based on freshness of the output, to determine if it's been updated or not. And ofcourse a structured JSON output, to maintain consistency across runs.

**Parallel work:** Run 07 again. The split saved about two minutes of a fifty-four minute run and the two integrator passes cost sixteen between them. Sequentially it's faster. What I got out of it was evidence about coordination, not speed.

→ `run-and-cost-log.md`

---

## 5. Failure and next step

Walking into a hidden location from the north, nothing triggered and nothing showed up, but the player got stopped like something was there. While the map showed empty grass.

It passed all six acceptance criteria, the integrator, and the codex review with its fifteen deliberate breakages. A6c only says whether the _location_ is drawn, it never says anything about the barrier being visible. So the criterion was satisfied and the game was still wrong. This was an issue with the acceptance criteria for the UI, which imo is an open sided task and not really bounded, because we can't define objective taste, it changes based on what the result looks like and needs a human in the loop for feedback. Although Fable as a critque has good taste, as evident from okiya game.

Same with the hidden location itself, it shows an empty area in map until it's filled, which isn't really correct since in games, the hidden locaiton is usally gated by bushes/treelines/hidden pathways etc.

How I caught it: by playing it, from a direction logic had not tested.

One adjustment for next month: run all three grilling directions instead of the two that are convenient. I did the interview and the interrogation, and skipped "challenge the design", the one that asks for failure modes.

→ `acceptance-and-evidence.md`, the escaped-defects table ·
`evidence/day-5-routines-and-capstone.md` · `next-steps.md`

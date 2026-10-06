# Running log

Written as I went, trimmed afterwards. Run-state noise ("agent is done",
"reviewing now") is cut; decisions, surprises and mistakes are kept, mostly in
the words I wrote at the time.

---

## Sep 21 — before starting

Read the whole program document first, so I wouldn't find out on day 5 that I
needed a report. Started this log for the same reason.

Upgraded the Claude plan, bought the GPT plan.

Ran a separate session to **audit my own config for prompt-injection
opportunities** before adding tools to my Mac. I'm about to give things access;
I'd rather look first.

### On the reading

Simon Willison's GIF-optimisation piece — what made it work was the author
knowing what he wanted, the project being small enough for the model to hold
entirely, and his own taste and experience. He logs his optimisations. I don't:
optimising is second nature for me but I never record what I optimised.

> Coding agents work so much better if you make sure they have the ability to
> test their code while they are working.

Agreed, and it worked well on the project where I had it. The one regret is not
building that into the older hardcoded app, but that predates agents being this
capable.

Steinberger's blog — I check who's writing and why. He built openclaw, so he has
reason to speak well of agents. Reading it open-minded but not uncritically.

The capability-curve video: Amara's law applies. I'm not a fan of the daily hype
cycle where yesterday's gold is today's trash. The speaker is from Anthropic, so
there's a bias in what gets pitched. Watched it anyway.

What was actually in it: feedback loops, plan → execute → verify → adjust, write
evals around your work, write prompts for intent rather than around model
weaknesses. Nothing new, but the feedback-loop point is the real one.

**The post that linked the video misrepresented it entirely** — it claimed the
talk was about graphs; it wasn't. Engagement farming, manufacturing FOMO.

## Sep 24 — picking the project

Built the map in Tiled. Cozy Valley: top-down, walk around, meet an NPC, Japanese
greeting dialogue. Stretch goal: custom player.

Slices written out, boilerplate filled.

## Sep 25 — the A/B

Ran attempt A with my old prompting approach, logged the evidence, then took a
break so I wouldn't carry thoughts from A into B.

Attempt B: disabled engram so memory isn't leaked. There's also a workspace skill
that reads from the shared vault — have to account for that. No grilling here,
since grilling is my old approach and isn't part of the new one.

B used Playwright MCP to play the game and inspect it. Fewer tests written this
time. It was aware of the previous attempt's branch.

Player faced opposite to movement direction; diagonal movement jittery. Gave
feedback, it fixed it.

Deliberately **not** reading the wall of text it returned, to keep it comparable
to how I handled A. Just a quick look.

> Confidence in the code is low, because I wasn't grilled here, so all decisions
> about structure are from agent itself.

Held-out cases: 7 of 7 pass.

### Codex on the same brief

Got the feet hitbox right on its own. No Playwright MCP.

It made the UI better unprompted — not fullscreen, a proper page with the game in
it and the controls written on the page. Added a "show hitboxes" toggle, a "back
to start", and movement instructions. Added the animation and map boundary. None
of the jitter B had.

Held-out cases: 7 of 7. Accepted.

> One thing that was good is that it added things on its own and they looked
> good. Whereas attempts with Claude added only what they were asked, or what I
> got from grilling. So this model has shown me first signs of taste.

**Open question I never resolved:** does telling the model a time limit make it
cut corners? Can it even sense time?

---

## Sep 27–28 — grilling, and deciding what not to review

Watched the grilling material. I already use grilling. On handoff I have my own
flow with engram, the Obsidian workspace and a handoff skill.

The author says not to read the spec written after grilling, since you already
share the understanding. Extending that: if I already green-flagged the idea, I
don't need to review the implementation — unless it's critical.

### From the design reading

- Layout constraints → propose a set of solutions → add or remove constraints as
  the design changes.
- Don't play whack-a-mole. Collect the symptoms, check whether each is downstream
  of a design constraint, then revise the constraints together. Often a larger
  thing surfaces and fixing that fixes the rest. This could be evident in cases like on foresight PoC with the modal showing with predictions in the same page, the logic was also affected by that, had we done a new page after clicking "Predict all" we could test that page in isolation and have it loaded dynamically to reduce the first load on user. So, design translate to code complexity. It's good to see your choices and choose which area to lean more on , the UX or complexity. Sometimes giving up a bit of UX to get huge gains on reduced complexity and better testability is worth it.
- Don't prototype design inside the production codebase — the agent grafts off
  what's already there.

### From the agentic code review reading

- Review need varies: a solo dev needs tests more than review; a team on legacy
  code needs both.
- With agent code you lose the reasoning. The reviewer only sees the artifact and
  has to work out why something is there and why alternatives were discarded.
  **Idea: attach the grilling session to the PR.** Keep a decision log.
- The reviewer shouldn't be the first person to put eyes on the code.
- Another AI should review the AI — but whether a change is needed at all stays
  human.
- **Human in the loop is a dial. Adjust it on blast radius.**
- Writing code is almost solved. Reviewing isn't, and understanding stays
  expensive.

### Slice 2

Added a line to the brief asking whether the agent knew about the handoff
documents. It worked — it printed the decisions it was working from and properly
referenced the ADR docs and engram.

> This project doesn't have a money-costing feature, so for review I'll look at
> tests instead of the actual written code.

It ran the e2e tests five times each to catch flakiness, listed the commands it
ran, and stated open risks.

The dialogue ended up at the top. I'd have preferred the bottom, but we'd decided
top during grilling and it turned out better.

### The interrogation afterwards

> What evidence supports completion? List the commands actually executed and
> their outputs, and any untested behaviour. Which check would fail if your
> implementation were wrong? Name one assumption I should independently verify.

It started **mutation testing on its own** — breaking things and confirming the
tests went red. Found one behaviour covered by a browser test and no unit test.
Listed the commands it ran, in order.

### Interface alternatives

Pushed back on doing this in-repo: the agent bases the design off what exists,
and I can't give feedback by clicking the interface the way I could elsewhere.

Six variants. Implemented one with a better cloud style. The change was confined
to two files, it ran the checks, found one broken test and updated it — a genuine
break caused by the layout.

Handoff was already practised in the grilling → build session, so there was
nothing new to test there.

---

## Sep 29–30 — skills, loops and goals

From the long-horizon talk:

- **Separate the agent doing the work from the one judging it.** Applies
  everywhere, including to humans.
- Workspace notes act as the global context. They should be **append-only**, with
  stale entries marked and referencing what replaced them.
- **Idea: a zettelkasten for agents.** Write the session to short-term memory
  first; when the goal is reached, process it into long-term. Tag each memory
  with where it came from, the mood, the confidence, whether it's a fact or a
  rationale. Name each link so it can be traced or skipped. The more a memory is
  recalled, the deeper it sits; what isn't used decays.
- Things I could route through loops: PRs and open issues into my todos; CI
  watching while I do something else; comments I leave while working turned into
  issues, then a workflow over the non-critical ones, merged on confidence, with
  the critical ones reviewed by me.

Made the verify-done-claims skill, global and repo. Went back through it to
remove unnecessary lines — there was nothing to remove, every line carried
weight.

### The worker + loop run

I wanted the loop agent to verify closed tasks as they come in and give me
confidence scores, so I'm not in the loop myself.

The loop caught the first commit, which was only a prettier fix, and identified
it as such.

> The loop caught the commit, but since the worker agent hadn't moved the issue
> to /closed, it skipped the check. This is an issue.

It was watching `issues/closed` in the repo rather than the workspace notes.

Couldn't catch the worker mid-flight to break a test — too fast. So I commented
out a line in a file it had already touched. **It caught it, and asked me rather
than fixing it.** I removed the line again before it committed; it committed, ran
the tests, found the failure, and fixed it.

Asked it to run tests in subagents, because checking them itself meant it
couldn't keep up with the worker's commit rate.

It correctly attributed expected red checks to an earlier commit rather than
re-diagnosing them:

> Commit `e000fd7` draws no cloud at all since the `fillOutlined(...)` call was
> dropped before that commit — matching the SHA in the issue's `fixed_in`, so red
> checks are expected there. I'll flag this to the subagent so it doesn't waste
> time diagnosing the missing cloud itself.

The loop stopped because I'd stated a limit in it:

> These closed issues arrived after the limit and weren't verified.

Restarted with no limit and a stop condition of "no new commits in 5 turns".

Then the worker made another commit after the loop was down. **I need better read
criteria — in practice that's GitHub PRs and issues tracked against commits.**

Notes from watching it:

- With each commit, did it update the docs? Check the blast radius? Log the
  change to engram or the workspace notes?
- The loop could comment on PRs or issues directly, with evidence and a severity.
- The issue files only get appended under "Verification", not edited above — good.
  Better still: append with timestamps and reference the superseded version.
- There's a lot of output in those files now. That makes it likely I won't read
  them at all, which defeats the point.
- It should attach images where they'd help.

Afterwards I asked it four questions: did you edit or remove anything under
`/closed`; did you see odd file changes from another agent; did you change repo
files; did you have knowledge from engram or workspace notes. **Clean on all
four** — appends only, aware of the other agent, no repo changes, initial engram
context and nothing after.

**Side idea:** bundle PRs by cognitive load rather than by relatedness. Or a mix.

### End of day 3

I'm barely reviewing code myself now. With the loop run I just did a visual
check. That holds here because this is a personal and ambitious project where I
need the tests to be right more than I need to read the diff. On something with
money at stake I'd review the critical paths and manually test before accepting.

The harder thought: I'm getting distant from the code. My speciality is hacking
around things, connecting dots, making things work. If I don't know the code, how
do I connect the dots? I think I now have to reason in features and a vague sense
of architecture rather than in code.

It takes the same time to understand the codebase either way. I can push tasks
faster, but understanding still costs what it costs. **I'm the one who can ask
"why". The agent just makes changes.**

What if the point isn't 5x or 10x, but offloading the less important work so
there's more mental room for the decisions that matter.

---

## Oct 1 — graphs, cost, and the first real workflow

### On workflows

Useful shapes from the docs: find flaky tests by running the suite repeatedly and
stopping once two rounds find nothing new; review every changed file in a PR, then
merge the findings into one ranked summary.

Subagent doctrine I settled on:

- Use one when the task floods context with files and searches I won't need again.
- Delegate high-output work: documentation reading, log processing, file review.
- A subagent could load engram first and return only what's needed.
- Fork when the task needs the same context; a fresh subagent starts clean.
- Use cheaper models for searches inside a workflow.

**What works for me is knowing the tools of the trade — after that I can join
them up myself.** Which suggests a workflow whose job is to go find new tools
worth knowing, for my work and for AI.

### On cost

- Prompt caching is the best quick win. Already on for me, and usage dropped after
  I turned it on.
- Upload files through the files API rather than pasting.
- Batch API suits issue triage and note gathering.
- Two patterns: advisory (cheap model does the work, expensive one advises) and
  orchestrator (frontier on top, cheaper models on small tasks).
- Asking the model to finish quickly mattered, and it performed well.
- **Decision: don't optimise cost yet.** Get the workflow running, then cut, so I
  can see what actually gets hit.
- Audit my own harnesses — engram, workspace notes, the verifiability step, the
  blast-radius skill. Right now every prompt goes through an engram search first.
  Should it?

### The graph

> There's one flaw with this graph: sometimes the core changes based on the other,
> and here we're keeping them separate. What if the UI requires a completely
> different method on the core and the core doesn't expose it?

First attempt: I ran the two workers as separate sessions rather than one
orchestrated workflow. That was wrong, and I redid it.

Logic worker finished almost immediately. **The split was too small to be
effective here.**

Restarted from scratch with the workflow, memories cleared and worktrees removed.

I broke worker 1's tests deliberately and committed (`10dbddb`), then resumed.
**It caught the drift and held for my decision:**

> I found a new commit (10dbddb) that landed after worker 1's reported SHA, which
> weakens the health clamp from MIN_HEALTH to -1 and breaks the "clamped to
> 0..100" contract — something the integrator prompt as written wouldn't catch
> since it never compares the reported SHA against the branch head.

I chose to gate the branch head. That makes it an intervention rather than a
resume.

### What the visual check found

The village greyed out and the fence and crops were removed. But the obstacles
are still there — **the fence is gone and I still can't walk through it.**

It also doesn't survive a reload, which I'd mentioned but not in the workflow
prompt, so that one's fair.

**No engram calls in the workers**, though the main agent did invoke it. And a
hardcoded asset path survived into the output; I had it fixed.

### From the reading, on someone else's workflow

He isn't working on anything critical — he plays with it and sees how it feels.
That doesn't transfer to a team project where money is at stake and values have
to be parsed correctly.

He doesn't checkpoint, so I won't either. Asking the agent to fix it and moving on
beats handling resume and time travel.

Structure projects _for_ the agent rather than against it. I've been fighting it a
bit by keeping workspace notes global instead of in the project. Propagate patches
and patterns across projects agentically and review on return. Refactor
aggressively. Build the core and a CLI first, so the next agent has a tested API
to work against.

### On running things in parallel

The independent side task should be **non-cognitive** — small routines, things
that don't need me thinking — so the main part of my brain stays on the workflow.
A workflow could maintain that list of side tasks and I tick them off.

---

## Oct 4–5 — routines, a wrapper, and the capstone

A practical brief covers: the outcome, the context, the constraints, the
non-goals, the acceptance criteria, the integration notes (which files are
off-limits, where the seams should be), and a verification plan.

On running several sessions at once: account for your own capacity. Don't take on
so much that you keep going back to check whether something broke silently. Keep
decision-making on one task, bounded enough to review. Notice what's draining.

> Try scope reduction before count reduction. Tighter scope per thread reduces the
> mental overhead each thread carries, which raises how many you can run while
> staying genuinely on top of the work.

> Supervision without understanding is exactly where comprehension debt lives. The
> code ships, but your mental model falls further behind with each thread.

That matches what I'd been feeling. The answer for me: keep the scope small
enough that I still know what I'm doing, apply a workflow inside that scope, test
visually, review only the critical code.

**Routine idea:** have an agent triage issues into what needs my attention and
what doesn't. Bounded tasks go on a list with a tracker for what an agent is
currently on, so I keep clearing the queue on the side while my main task holds
my attention. Open question: will I develop the skill to identify which tasks are
boundable, or only recognise them after the fact?

### Day 5

Weekly limit at 38%, but a side project is sharing the account, so there's no
point analysing it.

CLI wrapper done. First triage run asked for permissions while running in the
background where I couldn't approve it — reran in the foreground. Updated it with
no MCP, strict tools and strict directory access.

Run 2 was contaminated by run 1's output, so I redid both.

> From comparing run 1 and 2, it's clear that prose can be different, so we need
> to make sure structure at least is the same — and we do that by specifying the
> output format in the contract, so at least that's identical. Semi-idempotent.

Changed one issue and ran it again.

Capstone from 17:06. Started the held-back task at 18:30.

> There are some things visually that aren't right and I'd likely update, but
> that's more on taste than the code.

The first of those turned out not to be taste: approaching a hidden location from
the north, the player is stopped by a barrier drawn as nothing. It passes every
acceptance criterion I wrote.

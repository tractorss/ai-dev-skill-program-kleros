# Day 4 — graphs, cost and the first real workflow

**Oct 1.** Slice 3 behind a shared contract, two workers, an integrator.

## What I did

- Committed the contract first (`77e138c`) — types and signatures only. Both
  workers branched from that revision.
- Ran logic and UI as parallel workers with an integrator owning acceptance.
- Broke a worker's output deliberately mid-run and resumed, to see what the
  orchestrator would do.
- Read the cost material and decided what to act on.

## What happened

**First attempt was the wrong shape.** I ran the two workers as separate sessions
rather than one orchestrated workflow. Scrapped and rerun properly.

**The split was too small to be worth it.** The logic worker finished almost
immediately while the UI worker was still going.

| Node       | Effort | Wall   | Output | Tool calls |
| ---------- | ------ | ------ | ------ | ---------- |
| Logic      | medium | 2m15s  | 2.7k   | 22         |
| UI         | high   | 10m13s | 11.2k  | 68         |
| Integrator | high   | 6m48s  | 10.6k  | 58         |

Elapsed 54 minutes against a 90-minute limit. 388,578 tokens for the resumed run,
excluding orchestration.

**It caught the drift I planted.** I broke the health clamp and committed
(`10dbddb`), then resumed:

> I found a new commit (10dbddb) that landed after worker 1's reported SHA, which
> weakens the health clamp from MIN_HEALTH to -1 and breaks the "clamped to
> 0..100" contract — something the integrator prompt as written wouldn't catch
> since it never compares the reported SHA against the branch head.

I chose to gate the branch head, which made it an intervention rather than a
resume.

**The visual check found what no test did.** The village greyed out, the fence and
crops were removed — and the obstacles were still there. The fence is gone and I
still can't walk through it.

**Context didn't reach the workers.** No engram calls in either worker, though the
main agent made them. A hardcoded asset path survived into the output.

## What I learned

**I spotted the seam flaw before running it:**

> What if the UI requires a completely different method on the core and the core
> doesn't expose it?

The logic/UI split assumes a stable seam. That assumption isn't free.

**The split bought coordination evidence, not speed.** Two minutes saved on a
fifty-four minute run, against sixteen minutes of integration. Sequentially it's
faster.

**Pausing is expensive.** Two pauses threw away about 108k fresh input tokens of
UI work. The second lasted four seconds and still burned 18.8k.

**Per-node effort held up.** Logic at medium was right first time; UI at high was
working against real ambiguity. I'd predicted that routing the same morning with
no evidence — this is the first run that supports it.

## On cost

- Prompt caching is the best quick win; already on, and usage dropped after.
- Upload through the files API rather than pasting.
- Batch suits triage and note gathering.
- Two routing patterns: advisory (cheap does, expensive advises) and orchestrator
  (frontier on top, cheaper below).
- Asking the model to finish quickly mattered, and it performed well.
- **Decision: don't optimise cost yet.** Get the workflow running, then cut, so I
  can see what's actually being hit.

## On subagents

Use one when the task would flood context with files and searches I won't need
again. Delegate high-output work — documentation reading, log processing, file
review — and take back only the summary. Fork when the same context is needed; a
fresh subagent starts clean.

## On other people's workflows

The practitioner I was reading isn't working on anything critical — he plays with
it and sees how it feels. That doesn't transfer to a team project where money is
at stake and values have to parse correctly. He doesn't checkpoint, and I've
decided not to either: asking the agent to fix it and moving on beats handling
resume and time travel.

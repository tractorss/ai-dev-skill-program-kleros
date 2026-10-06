# Day 2 — grilling, and deciding what not to review

**Sep 27–28.** Three grilling sessions, slice 2, interface alternatives, handoff.

## What I did

- Three grilling sessions before implementing the NPC dialogue slice.
- Added one line to the brief asking whether the agent knew about the handoff
  documents.
- Implemented slice 2, then interrogated it about its own completion claim.
- Asked for six interface alternatives, implemented one (`feec14a` — 2 files,
  +157/−89, about ten minutes excluding the alternatives).
- Practised handoff, which the grilling → build session had already covered.

## What happened

**The handoff line worked.** The agent printed the decisions it was working from
and cited the ADR documents and engram, instead of starting from the brief alone.

**The interrogation triggered mutation testing unprompted.** I asked:

> What evidence supports completion? List the commands actually executed and
> their outputs, and any untested behaviour. Which check would fail if your
> implementation were wrong? Name one assumption I should independently verify.

It broke its own code, confirmed the tests went red, found one behaviour covered
by a browser test and no unit test, and listed the commands it had run in order.

**The dialogue ended up at the top** of the screen. I'd have preferred the bottom,
but we'd settled on top during grilling and it was the better call.

**The alternatives ran in-repo, which I pushed back on** — the agent bases its
design on what already exists, and I couldn't give feedback by clicking the
interface.

## What I learned

**Review depth is a decision, made before the work, based on consequence:**

> This project doesn't have a money-costing feature, so for review I'll look at
> tests instead of the actual written code.

That became the rule for the week. On something with money or users at stake I'd
review the critical paths and manually test before accepting.

**The reviewer shouldn't be the first pair of eyes on the code.** Another AI does
the first pass; whether a change is needed at all stays human.

**Human in the loop is a dial, set by blast radius** — not a switch.

**Agent code loses its reasoning.** The reviewer sees the artifact and has to
reconstruct why it's there and what was discarded. Two fixes worth trying: attach
the grilling session to the PR, and keep a decision log.

**Don't prototype design in the production codebase.** The agent grafts onto
what's there. Collect the symptoms rather than fixing them one at a time, check
whether each is downstream of a design constraint, then revise the constraints
together.

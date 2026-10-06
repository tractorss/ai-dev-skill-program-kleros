# Day 3 — skills, loops and goals

**Sep 29–30.** Built a skill, tested its trigger, ran a worker with a verification
loop beside it.

## What I did

- Wrote the `verifying-done-claims` skill and committed it (`3d7c883`), global and
  repo. Went back through it to cut unnecessary lines and found none.
- Tested the trigger with prompts that should and shouldn't fire it.
- Ran a worker agent on a goal, with a loop agent beside it verifying commits as
  they landed, so I wouldn't be in the loop myself.
- Interrupted the run deliberately to see what the loop caught.

## What happened

**The trigger was too narrow, not too broad.** I'd predicted the opposite. It
keyed on a claim existing rather than on being asked to verify one, so "is the
work complete?" didn't fire it. "Check if the work was done correctly" did.

**Delegation worked, on one condition.** The skill argues against handing the
procedure to a subagent, because a subagent's tool output never reaches you. The
agent delegated anyway. The result was still real evidence, because the
orchestrator required the skill's report format — verbatim excerpts and exit
codes — as the subagent's final message.

The skill now permits delegation on exactly that condition:

> A subagent may run it, and doing so works — but only on one condition: its
> final message _is_ the report below, carrying verbatim excerpts, exit codes and
> per-criterion mutation results. A subagent that returns conclusions ("all
> checks pass", "the layout is correct") has handed you the claim a second time,
> one level deeper.

**The loop failed on its read criteria, not its logic:**

> The loop caught the commit, but since the worker agent hadn't moved the issue to
> /closed, it skipped the check. This is an issue.

It also stopped while the worker was still committing, because I'd stated a turn
limit. Restarted with "no new commits in 5 turns" instead.

**It handled a live break well.** I couldn't catch the worker mid-flight, so I
commented out a line in a file it had already touched. The loop caught it and
asked me rather than fixing it. I removed the line again before it committed; it
committed, ran the tests, found the failure and fixed it.

**It distinguished expected failures from new ones** — attributed red checks to an
earlier commit via the issue's `fixed_in` SHA and told the subagent not to waste
time diagnosing them.

**I interrogated it afterwards** with four questions: did you edit anything under
`/closed`, did you see odd changes from another agent, did you change repo files,
did you have engram or workspace knowledge. Clean on all four.

## What I learned

**Separate the agent doing the work from the one judging it.** My own note that
morning, before the loop existed. It applies outside software too.

**Read and stop criteria belong on durable shared artifacts** — PRs, issues,
commits — not on a local convention like a folder move. A loop keyed to a
convention silently stops verifying when the convention isn't followed.

**Completion conditions, not repetition caps.** The one limit that bound all week
was this loop's turn cap, and it's the one that caused harm.

**Append, don't edit.** With timestamps, referencing what was superseded. And
watch the volume — the issue files got long enough that I'd stop reading them,
which defeats the point of writing to them.

## Ideas worth keeping

**A zettelkasten for agents.** Write the session to short-term memory first; on
reaching the goal, process it into long-term. Tag each memory with its origin, the
mood, the confidence, and whether it's a fact or a rationale. Name each link so it
can be traced or skipped. Memories recalled often sit deeper; unused ones decay.

**Bundle PRs by cognitive load** rather than by relatedness. Or some mix.

## The honest part

I'm barely reviewing code myself now. On this project that holds — I need the
tests to be right more than I need to read the diff. But I'm getting distant from
the code, and my speciality is connecting dots and hacking around things, which
needs knowing what's there.

Understanding the codebase costs what it costs. Shipping faster doesn't reduce it.
I'm the one who can ask _why_; the agent just makes changes.

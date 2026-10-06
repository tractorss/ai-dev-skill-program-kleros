# Operating plan

---

## Allocation

- **Primary model:** Claude
- **Reviewer:** Codex And claude (Claude included again for better reviews from two agents, one usually finds issue the other doesn't)

## Routing by task

- **Logic, well-specified:** Depends, Opus medium for grilling has been sufficient, Opus/Sonnet (Medium/low) for implementation, Fable High as advisor
- **UI and ambiguous work: Opus medium for grilling, Opus medium for pre research, Fable high as designer, Fable medium/high as critique, Opus/Sonnet medium/low as implementer**
- **Integration and verification: Opus High/medium**
- **Specification vs implementation: Opus medium for grilling, Opus/Sonnet medium/low for implementation based on the slice complexity.**

Run 07 and the Okiya build are the evidence here — see `run-and-cost-log.md`.

Aside from above, I am experimenting with **Foreman**, a personal manager that combines various things on a unified interface to help me go from notes/discussions to issues to finished tasks. Since during our day we can have several notes from discussions and open issues, PRs to review, infra tasks, security tasks. They hog mental space. This is an effort to combine my various note taking sources, github, slack and use those notes/discussions to create issues. Those issues are then triaged and depending on the type of task, they may be fully assigned to an agent, or it would be assigned to me + agent.  
The complete app has more features that are beyond the scope of this operating plan to explain. Ping me to know more about it.

## Project rules vs reusable procedures

- What belongs in a repo's own rules: Project architecture rules, ex: with foresight I had bulletproof react web skill, that is invoked on each run on the web app and guides the agent on where to place the files. It also helps the agent to quickly find the related data.
- What belongs in a portable skill: Usually everything that is not project specific, something I find myself repeating, one of them being a blast radius check after each session, to make sure targeted changes do not silently drift the logic or leave behind dead code.

## When coordination is worth it

Run 07's split saved about two minutes of a fifty-four minute run and cost
sixteen minutes of integration. So:

- **Worth splitting when:** Task has clear boundaries between the splits and there is no reliance from one worker on another. A good example is splitting an app's new feature by logic and UI, and occasionally indexer/subgraph, with their own tests. Followed by an integrator/reviewer/critique/red-team(flaky) depending on the type of task. For other cases, having a quick ask to another agent to get a good split/workflow is usually worth it.
- **Not worth splitting when:**
  The task is small and bounded, in that case subagents should be more than enough. Or when you need deep research first, that becomes sequential. Bug fixes don't fall under workflow either.

## Required acceptance evidence

Before I accept a change, I need:

- [x] the command that was run, and what it printed
- [x] a check that fails when the behaviour is broken
- [x] If a visual change, an added critique agent's review, UI testing , manual testing.
- [x] If a bug fix, a proper report with patch, used attack vectors if a security fix
- [x] blast radius of the change

## Budget escalation

- Normal: 200$ Claude + 100$ Codex
- When a limit is hit: Switch to another account
- When I'd spend more: Depends, PoC are usually the most ambitious spends, to quickly materialize ideas. Or on Security reviews.

## Concurrency

- **Limit:** **One attentive task with 2-3 side tasks of low cognitive load and complexity** For this I would usually focus on one attentive task at the moment, and with the Foreman app, it would give me tasks that are completely handle-able by an agent and I would assign it to them. The goal is have multiple of those tasks given to the agents on side, with an assistant AI to manage them and I can focus on the tasks that require my attention. And once in a while, I can take a quick look at all the side tasks and approve them. This is still in testing and would improve with the feedback I get from its implementation. The goal is to build around the mental limits and not try to exhaust myself , which would then make a negative loop.
- **How I clear pending reviews before starting more:**
  Two types of reviews here, the one from active sessions I will alrd be in the loop, so it would be easy to review. The ones that will stack will be the on-the-side agents, for them I have a proper format for them to report back in, taken from the report. The assistant agent would give the first pass as a reviewer. Depending on the task load, it may be assigned a codex reviewer, and then based on task type I will perform quick checks. And mark it reviewed.

## Weekly maintenance

- What I check: Session costs, mental check-in (how much am I actually understanding of what I'm shipping), engram sanitization, memory sanitization, what am I repeating (to create a skill or workflow), New tools or optimization techniques, new model capabilities and effort level reassess, findings unique to one reviewer versus shared misses.
- **Next review date:** 25 Oct 2026

# Day 1 — baseline and the A/B

**Sep 21–25.** Setup, the resources, and the same task run three ways.

## What I did

- Read the whole program document before starting, so the reporting requirements
  wouldn't surprise me on day 5. Started a running log.
- Audited my own config for prompt-injection before adding tools to the machine.
- Picked the project: Cozy Valley, a top-down game for learning Japanese grammar.
  Built the map in Tiled. Five slices written out.
- Ran the first slice — player walks the map, respects collisions — three times:
  attempt A with my old prompting approach, attempt B with the program's, and the
  same brief on the second provider.

## What happened

**Attempt A** — old approach, with grilling. Logged, then a deliberate break
before B so I wouldn't carry thinking across.

**Attempt B** — the program's approach, no grilling, engram disabled. Player faced
opposite to movement; diagonal movement jittery. Fixed on feedback. Fewer tests
written than A. Held-out cases 7 of 7.

I deliberately didn't read the response in full, to keep it comparable to how I'd
handled A.

**The second provider, same brief** — got the feet hitbox right unprompted, with
no browser-automation tooling available. Added things nobody asked for: a
non-fullscreen page with the controls written on it, a hitbox toggle, a
back-to-start, the animation, map boundaries. None of B's jitter. Held-out cases
7 of 7, accepted with no corrections.

## What I learned

**Grilling buys standing, not better code.** The line I wrote at the time:

> Confidence in the code is low, because I wasn't grilled here, so all decisions
> about structure are from agent itself.

Nothing in B was visibly wrong. The problem was that I had no basis for accepting
it — every structural decision had been made somewhere I wasn't.

**Taste is a real difference between models**, and it shows up as what gets added
unasked. One model added only what was asked or what grilling surfaced. The other
added affordances I'd have wanted and didn't specify, without the tooling to see
the result.

**Check who's talking.** I did this with every resource — the author's product,
the speaker's employer, the stakes they're working under. One post about a talk
misrepresented it completely. Same instinct as not accepting an agent's claim,
applied to people.

## Open question

Does stating a time limit make a model cut corners? Can it sense time at all?
Raised here, never tested.

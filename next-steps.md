# Next steps

Short note, written at the end of the week. What's parked, what's unfinished, and
the one thing I'm changing.

## Unfinished

|                                | How far it got                         | What remains                           | Blocker                                                                       |
| ------------------------------ | -------------------------------------- | -------------------------------------- | ----------------------------------------------------------------------------- |
| Slice 4 — worksheet boss fight | not started                            | the whole slice                        | none; the week never scheduled it                                             |
| Slice 5 — end state            | not started                            | the whole slice                        | same                                                                          |
| Capstone — sound and animation | cut at 17:10 on day 5, before building | audio on discovery, a reveal animation | deliberate scope cut, recorded before the build rather than after it ran long |

## Known defects, unfixed

- Approaching a hidden location from the north, the player is stopped by a
  barrier drawn as nothing. Passes all six acceptance criteria.
- The player is painted over tall props instead of being occluded by them.
- `parseHiddenLocations` treats the bounding box of all barriers as a ring, so it
  rejects valid open geometry.
- The sabotage commit from day 4 and its revert are both still in master history.
  Squash before anyone reads that log.

## Fixes queued on the week's own tooling

- **Give `triage.sh` a format contract** — a schema over `{issue_id, priority,
reason, evidence}`, with markdown as a rendering of it. Without it nothing can
  consume the output and a diff of it is all false positives.
- **Put the priority rubric in the prompt.** Today P1–P3 is a relative sort
  printed as absolute labels, so editing one issue moves another issue's label.
- **Drop `Edit` from the routine's tool allowlist.** It writes one file; `Write`
  is enough, and it moves a permission clause from prose to flag.
- **Pin the routine's real input.** The repo working tree is an undeclared input
  — the output depends on which commit is checked out.
- **Fix the workflow script's verification step**, which names a test runner this
  repository doesn't use.

## Parked, with a reason

| Idea                                                                                                                                                                                      | Why it's parked                                           |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| An issue-to-PR pipeline — comments I leave while working become issues, non-critical ones run through a workflow, critical ones I review                                                  | needs connectors that aren't installed                    |
| An attention-triage dashboard — gather tasks from the apps I use, show them with references, tick them off                                                                                | the hard part is dedup and deciding what counts as a task |
| A zettelkasten for agent memory — short-term first, processed into long-term on goal completion, each memory tagged with origin, confidence and type, links named, unused memory decaying | an idea, not a design                                     |
| Bundling PRs by cognitive load rather than relatedness                                                                                                                                    | untested                                                  |
| One representative task at two effort levels, measured                                                                                                                                    | the A/B I kept deferring                                  |

## Open questions

- Does stating a time limit make a model cut corners? Can it sense time at all?
- Can the skill of spotting a boundable task be developed deliberately, or only
  recognised after the fact?
- Should every prompt go through a memory search first?

## One adjustment for next month

_To pick._ Four candidates:

1. A criterion about a set gets checked against more than one member.
2. Boundaries get enforced by tool access, not by sentences.
3. Routines get a format contract, not just a path and a content contract.
4. **Run all three grilling directions, not the two that are convenient.** I
   interviewed and I interrogated. I skipped "challenge the design", which is the
   one that asks for failure modes, and it's the step with the best chance of
   catching the invisible wall before it was built.

## Cleanup, end of week

- [x] Scheduled jobs inspected — none were wired; the routine is invoked by hand
- [x] Temporary credentials revoked — none were created
- [x] Extra worktrees removed
- [x] Actual spending reconciled
- [x] Secrets confirmed absent from committed files
- [x] Capstone branch merged, or recorded as deliberately unmerged

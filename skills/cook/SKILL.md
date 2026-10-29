---
name: cook
description: Use when the user runs /cook <ticket> or asks to take a ticket from intent to a verified draft pull request without supervision. Runs the agent kitchen, which covers intake, implementation, verification and hand-off, using the project's mise-en-place skill.
argument-hint: <ticket-key> followed by the ticket content, if the project expects it pasted
disable-model-invocation: true
---

# Cook

You are the executive chef of the kitchen. You take one ticket from intent to a verified draft pull request. You
coordinate the work, but you never cook yourself:

- a **line cook** implements;
- a **taster** verifies;
- the user is the chef, and is involved exactly twice: approving the brief (Gate 1) and reviewing the draft pull
  request (Gate 2). Everything in between runs without them.

Trust comes before scale. One run that is reliable, verified and honest beats several fast ones that the user has to
babysit.

Input: `$ARGUMENTS`. The first word is the ticket key. Everything after it is ticket content, if the project expects
the ticket to be pasted.

## The project's mise-en-place

Every project describes its own kitchen in a project skill named `mise-en-place`. Load it before anything else. Its
sections are your configuration:

| section | what it tells you |
| --- | --- |
| Ticket intake | where the ticket content comes from, and the ticket key format |
| Preflight | the checks that must pass before Gate 1, each with a pass condition and a fix |
| Workspace | how to create the isolated workspace (worktree path, branch naming) and prepare it (install steps) |
| Environment | how to start and stop the app, and the singletons: resources that exist once per machine, each with a free check and a maximum hold time |
| Validation | the commands the line cook runs before every commit |
| Verification | the skill the taster runs, and anything it needs to know |
| Conventions | commit and pull request titles, target branch, pull request body format |
| Never run | commands that are forbidden in the kitchen |
| Agents | optional, harness-specific worker definitions to use for the line cook or the taster |

If the project has no `mise-en-place` skill, stop. Show the user `references/mise-en-place-template.md` and offer to
draft one from the repository. Never guess a project's environment.

## Rules

- Never edit product code, tests or configuration yourself. Every change goes through the line cook.
- After Gate 1 the user is not asked anything. Decide, write the decision and its reasoning to `decisions.md`, and
  continue.
- Never push to the default branch, never force-push, never merge, never mark the pull request ready for review.
- Never run anything listed under **Never run**. This applies to every worker too, so pass that list to them.
- If you have to stop after Gate 1, still do the hand-off (Step 8) with whatever exists. The user should come back to
  a report, not a silent terminal.

## Run log

All state lives outside the repository, in `~/.kitchen/<project>/<TICKET>/`. `<project>` is the name of the main
checkout's directory, which stays the same when you run from a worktree:

```bash
PROJECT=$(basename "$(dirname "$(git rev-parse --path-format=absolute --git-common-dir)")")
```

The files are:

| file | content |
| --- | --- |
| `brief.md` | the approved brief |
| `decisions.md` | every decision taken without the user, with its reasoning |
| `round-<n>.md` | the taster's full report for round n, unchanged |
| `pr-body.md` | the pull request body |
| `run.md` | timeline: start and end of every step, time spent waiting for locks, rounds, final verdict, pull request url, **interventions** |

An intervention is anything that needed the user after Gate 1, or would have needed them. Examples:

- a permission prompt or a denied command;
- a missing credential;
- an environment failure;
- a question the brief could not answer.

Log each one in `run.md` with what happened and why. That list is how the kitchen improves, so never leave it out.
If `run.md` exists from an earlier run, show the user its state and ask whether to resume or start over. That question
still counts as Gate 1.

## Shared resources

Several kitchens can run side by side, each in its own session and workspace. Cooking runs in parallel freely. Some
resources exist only once per machine, for example a local stack with fixed ports or a shared test account. The
mise-en-place lists these as **singletons** in its Environment section, each with a check for when it is free and a
maximum hold time. Every singleton is guarded by a lock:

```bash
LOCK=~/.kitchen/$PROJECT/locks/<resource>
```

In the commands below, write the lock path out as a full absolute path instead of `$LOCK`, so that permission rules
can match the command.

- **acquire:** `mkdir -p ~/.kitchen/$PROJECT/locks && mkdir "$LOCK"` succeeds. This step is atomic. Then write
  `$LOCK/owner` with the ticket, the workspace path and the current time.
- **wait:** if `mkdir` fails, another run holds the lock. Wait with
  `until mkdir "$LOCK" 2>/dev/null; do sleep 60; done`, as a background command where the harness supports it, and
  then write the owner file. Record the wait time in `run.md`. Waiting is normal and is not an intervention.
- **stale:** take over the lock only when its owner is clearly gone, meaning either the owner's `run.md` records a
  finished hand-off, or the lock is older than the maximum hold time. Remove it, acquire it, and log an intervention
  naming the stale owner.
- **free check:** right after acquiring, run the singleton's free check. If the resource is still in use (a previous
  holder did not clean up, or the user is using it), wait 10 minutes and check again. If it is still in use, treat it
  as an environment INCONCLUSIVE.
- **release:** `rm -rf "$LOCK"` as soon as the resource is stopped. Release on every path out of a step, including
  failures and the hand-off.

Hold a lock only while you use the resource, which normally means the taste step. Never hold it while cooking, at
Gate 1, or between rounds, so other kitchens can taste while your line cook fixes findings.

## Step 1: Preflight

Run every preflight check from the mise-en-place before the user leaves. Report all failures at once, each with its
fix, and stop. Never continue past a failed preflight. A failure found now costs the user a minute. The same failure
found mid-run costs them the whole run.

A singleton that is busy because another kitchen holds its lock passes preflight, because this run will wait its turn.
A singleton that is busy without a lock belongs to someone outside the kitchen, and that fails preflight.

## Step 2: Brief

Read the ticket the way the mise-en-place describes. Then:

- pull every linked design (for Figma, use the Figma tools for design context and screenshots);
- read the code closest to the change.

Write `brief.md`:

```markdown
# <TICKET>: <summary>

Source: <ticket url>

## Goal
<one or two sentences>

## Acceptance criteria
1. <testable statement, phrased as observable behaviour>

## Design
- <frame name>: <design url with node id>

## Scope
- affected areas: <paths>
- feature flag: <name and how it is enabled where the taster tests, or "none: <reason>">
- out of scope: <bullets>

## Verification plan
- <page, flow or command the taster must exercise>
- <neighbouring features to explore>
- regression: <what runs and where, following the mise-en-place, e.g. "e2e regression: no (CI)">

## Open questions
- <question>: proposed answer <answer>
```

Every open question gets a proposed answer. The user confirms or overrides them at Gate 1, so nothing stays open after
it.

## Step 3: Gate 1

Show the brief and ask the user to approve it or name changes. Offer exactly two choices: **Approve**, and **Edit**,
which must carry the requested changes in the same answer (as free text, or as notes where the question tool has
them). If an Edit comes without its changes, ask for them in plain text instead of offering the choices again. Update
the brief and ask again until they approve. On
approval, tell them they can leave now and will be notified at the end. Then record the start time in `run.md`.

## Step 4: Workspace

Create and prepare the isolated workspace exactly as the mise-en-place says. All workers get its absolute path, and
nobody works in the user's own checkout.

## Step 5: Cook

Spawn the line cook with `references/line-cook-prompt.md`, filled with:

- the workspace path and branch;
- the brief path;
- the Validation, Conventions and Never run sections of the mise-en-place;
- the round number, and from round 2 on the path of the previous `round-<n>.md`.

In round 2 and later, continue the same line cook so it keeps its context. Spawn a new one only if continuing fails.
Append the decisions it reports to `decisions.md`, and log its blockers as interventions.

## Step 6: Taste

Spawn a **new** taster every round, with `references/taster-prompt.md`, so it judges the work without the cook's
reasoning. Fill in:

- the workspace path;
- the brief path;
- the cook's summary of this round;
- the Environment, Verification and Never run sections of the mise-en-place.

Give the taster no file-editing tools wherever the harness allows it. If the mise-en-place names an agent for the
taster, use that agent. Write its report to `round-<n>.md` unchanged.

Tools can leave evidence files behind: screenshots, snapshots, logs. The mise-en-place says where they land. Before
releasing the locks, move every evidence file from this round into `round-<n>-evidence/` in the run folder. The
workspace and the user's checkout must stay clean. Tastings take turns on the lock, so files from different runs
cannot get mixed up.

Acquire the lock of every singleton the taster needs before spawning it. Once the taster has returned, make sure the
resources are stopped and release the locks.

## Step 7: Loop

| taster verdict | next |
| --- | --- |
| PASS, PASS WITH FINDINGS | hand-off |
| FAIL | back to Step 5 with the findings, as long as n < 3 |
| INCONCLUSIVE because of the environment | taste once more if the cause could have cleared, otherwise hand-off |
| INCONCLUSIVE for another reason | hand-off |

A High or Critical finding caused by the change counts as FAIL, whatever the verdict says. A finding the taster marks
as pre-existing never triggers another round; it goes into the pull request body.

After round 3, hand off whatever the verdict is. A draft pull request with an honest FAIL is a useful result. More
rounds are not.

## Step 8: Hand-off

1. Make sure everything the workers started is stopped, as the Environment section describes, and that this run
   holds no lock any more.
2. Write `pr-body.md` in the project's format, then append these sections:
   - `## Verification`: the verdict, coverage and test counts from the last round, and the number of rounds;
   - `## Findings`: every open finding, highest severity first;
   - `## Decisions`: the content of `decisions.md`.
3. Push the branch and open a **draft** pull request, following the Conventions. Before the first push, squash the
   round commits into one clean commit titled like the pull request; nothing is pushed yet, so no force-push is
   needed. Never force-push. Once the branch is pushed, further changes are new commits.
4. Finish `run.md`: end time, verdict, rounds, pull request url and interventions (write `none` if there were none).
   Add a duration summary: total time spent on the brief, cooking, tasting and waiting for locks.
5. Notify the user (a push notification where the harness has one) with the ticket, the verdict and the pull request
   url.
6. Report the same in the session, together with the number of interventions and the path of `run.md`. Keep the
   workspace; the user may want to look at it.

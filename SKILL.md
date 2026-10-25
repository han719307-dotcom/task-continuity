---
name: task-continuity
description: "Manage task continuity and handoff for long-running, multi-phase, or multi-session work by maintaining a compact task card, choosing the right task boundary, and reconciling recovery against workspace evidence. Use when work must survive pauses, context compaction, session resumption, or thread handoffs, including migrations, refactors, research followed by implementation, and long verification. Do not use automatically for simple Q&A, one-command checks, or edits that finish in one turn."
---

# Task Continuity

Preserve the state that changes future action, not the conversation. A task card
is a working control surface: keep it short, evidence-backed, and current.

## Decide whether continuity needs management

Use the full workflow when the task includes any of these:

- work that spans more than one phase or checkpoint, where later actions
  depend on earlier results;
- a migration, refactor, release, or long verification likely to pause,
  resume, compact, or move to another session;
- research whose findings will drive an implementation and may need a
  handoff;
- recovery from an existing task card or handoff;
- an explicit user request to manage task continuity.

An explicit request to manage continuity takes precedence over these heuristics
and the exclusions below. Honor it even for a simple task, but keep the card
minimal. The single-turn exclusion applies only to automatic or inferred use.

File count alone does not require automatic continuity management. Do not create
a task card for simple Q&A, a single command, or an edit that will finish in one
turn, including a small multi-file edit that will not need recovery, unless the
user explicitly requests continuity. If an apparently small task becomes
multi-phase, start the workflow at the next checkpoint.

## Find the state carrier

1. Reuse an existing live plan, task, or progress file when it is already the
   project's source of truth and may be updated.
2. Treat user-authored plans as evidence unless the user allows edits; never
   overwrite them merely to impose this format.
3. If no suitable carrier exists, create one `TASK_CARD.md` in the current
   workspace. Keep using that file rather than creating one per phase. Give each
   active delivery a stable Task ID and use a separate
   `# Task Card: <task-id>` section for it. Reread the file immediately before
   updating and change only the current task's section; never overwrite another
   unfinished card.
4. If file writing is unavailable or unauthorized, keep the compact card in the
   conversation and state clearly that it has not been persisted. Before a
   pause or handoff, output the complete card in copyable form and say that the
   user must save or transfer it for continuity.

Do not create a memory database, background monitor, hook, service, or another
persistence layer.

## Task card

Keep the card concise enough to reread at every checkpoint. Omit narrative,
full chat summaries, whole command output, and repeated explanations.
Never store secrets, access tokens, private keys, personal data, or complete
sensitive logs in a task card. Use a safe file or source reference instead; for
commands, record the command, exit status, and necessary result summary.

```markdown
# Task Card: <task-id>

Updated: <date>
Status: active | paused | blocked | completed

## Objective and completion criteria
- Objective: <one delivery outcome>
- Done when: <observable, testable criteria>

## Current phase
<phase; include blocker or resume condition if paused>

## Completed with evidence
- <completed result> — <file, diff, test, or artifact evidence>

## Next action
- <exact next action, or condition required before work can resume>

## Decisions and constraints
- <decision or constraint that affects future work>

## Failed attempts
- <attempt> — <reason it failed> — <what not to repeat>

## Relevant files and artifacts
- <path> — <why it matters>

## Unverified risks
- <risk> — <how to verify or mitigate>
```

The title carries the Task ID. Always include Updated, Status, Objective and
Done when, Current phase, and Next action. Include the remaining sections only
when they contain actionable state; record material uncertainty under Unverified
risks rather than silently omitting it. Record verified completion separately
from work that was merely attempted.

On completion, set Status to completed and retain the objective, final evidence,
and artifacts. Set Next action to none and remove obsolete risks and failed
attempts. A completed card section may be removed once its result is no longer
needed; do not create an archive by default.

Update the card:

- after initial workspace inspection, before substantial work;
- when a phase or meaningful unit of work completes;
- after a test, build, review, or other result changes the delivery state;
- before context compaction, a long pause, or a task/thread switch;
- when new evidence invalidates a decision, next action, or risk;
- after recovery reconciliation.

Do not update it after every trivial command. A checkpoint changes future action;
otherwise leave the card alone.

## Choose the task boundary

Use the delivery objective, completion criteria, and dependency on current
working state, not the topic name.

- Same delivery objective and dependent on current state: continue the current
  thread.
- Same project but a new independent delivery objective: recommend a new thread
  with a new delivery card containing only shared project constraints and
  relevant context.
- Different project or unrelated task: recommend a new thread and do not carry
  the old task's process.
- Lookup, verification, or review in service of the current delivery: return the
  result to the main task. Use a subtask when its inputs, outputs, and result
  integration path are clear; do not delegate when a user choice is required to
  define the subtask's scope.

Never create a thread automatically. When isolation is appropriate, give the
user a compact handoff card ready to paste into the new thread.

## Hand off

Before proposing or preparing a handoff, reconcile the workspace as described
below. Then provide:

```markdown
# Handoff

Workspace: <path>
State carrier: <path, or "in-thread; not persisted">
Task ID: <same ID for continuation; new ID for an independent delivery>
Related Task ID: <only if this delivery depends on another task>
Deliverable: <objective>
Why this boundary: <same project new goal, different task, or clean isolation>
Done when: <completion criteria>
Current state: <phase and verified evidence>
Required context: <shared project decisions and constraints only>
Already tried: <failed attempts and reason, if any>
Next action: <first concrete action in the new context>
Relevant paths: <files and artifacts>
Open risks: <unverified items>
```

Generate a handoff from the task card's verified facts without introducing new
completion claims.

For a continuation of the same objective, preserve the Task ID and card section,
and carry the current working state, completion evidence, decisions, failed
attempts, relevant paths, risks, and next action.

For an independent delivery, create a new Task ID and card section. Carry the
workspace, shared project constraints, and relevant stable interfaces or
dependencies only. Omit the old objective, completion evidence, next action,
exploration trail, and task-specific risks unless they are genuinely shared.
Record a Related Task ID only for a real dependency; never reuse an old Task ID
for a new objective.

## Recover

Recovery begins with reality, not recollection:

1. Confirm the workspace, current files, changes, generated artifacts, and any
   available test, build, or verification evidence.
2. Distinguish task changes from unrelated user work. Never overwrite or claim
   uncommitted changes without evidence. Evidence can indicate likely ownership
   but does not authorize an overwrite, revert, or deletion; require explicit
   permission for those actions.
3. Then read the task card or conversation summary.
4. Reconcile every claimed completion against actual evidence. Treat a claim
   without a file, diff, test, or artifact as unverified.
5. If related implementation changed after a verification, treat the affected
   completion as unverified and rerun the smallest relevant check.
6. Continue from the smallest concrete action consistent with the verified
   state. Do not repeat exploration already supported by the evidence.

For factual implementation state, trust fresh workspace evidence over the card
or summary and correct the card. Current user instructions govern the objective,
completion criteria, and authoritative constraints; workspace state alone
cannot redefine them. Do not ask merely because the recorded phase is stale.
Ask when only the user can resolve a genuine objective or completion-criteria
conflict, provide required access or authorization, or disambiguate the
workspace or task identity.

If the workspace is missing or does not match the task, stop writes until it is
confirmed. If the card is missing, malformed, or has ambiguous Task IDs, do not
fabricate or merge state; reconstruct only from available evidence or ask the
user to disambiguate. If evidence cannot be obtained, mark the affected work
unverified or blocked and state what is missing. Distinguish failed tests from
tests that did not run or could not run.

If resuming in a new thread, do not ask the user to restate decisions that are
confirmed by a current task card, and do not trust that card until the recovery
checks above are complete.

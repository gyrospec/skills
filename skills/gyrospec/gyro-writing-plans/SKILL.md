---
name: gyro-writing-plans
description: Write a requirement-linked TDD implementation plan for a ready Gyrospec sub-ticket.
disable-model-invocation: true
---

# Writing plans

1. Call the Skill tool with "gyro". Follow its Node 20+ check, script path, and language rules. Reply in the user's language and keep convention keywords English. This workflow adapts Superpowers writing-plans; attribution is in the plugin repository's `THIRD_PARTY_NOTICES.md`.
2. Require arguments `<slug> <nn>`. Normalize a number such as `1` to `01`, then run `gyro status <slug> --json` before work. The named sub-ticket must exist, be `todo`, not stale, and have all `blocked_by` tickets done (`ready: true`), unless it is the revision case below. Require the current approved spec and its upstream gates to be unchanged and not stale, and require that `gyro check <slug> tickets` is clean; on any problem, show its lines and suggest `/gyro-breakdown <slug>`.

   **Revising an existing plan.** A sub-ticket that is `in_progress` or `blocked` and already has a plan file whose `Spec revision` is older than the current approved spec (after `/gyro-breakdown` reviewed it and moved its `spec_revision` forward, so it is no longer stale) gets its plan revised, not rewritten. Keep the plan's `**Execution start:**` and `**Out of scope:**` lines and the checked completed tasks with their evidence. Set `Spec revision` to the current revision, compare the old and new approved spec snapshots for the sections the sub-ticket implements, and mark each completed task that the change invalidates as unchecked with a one-line reason. Rewrite only the remaining and invalidated tasks against the new spec, by the rules in step 3. Do not change the sub-ticket's status. If a prerequisite fails, name the missing gate, dependency, or stale cause, suggest the correcting phase or reported `next`, and stop. Never repair gates or snapshots by hand.
3. Read `PLANNING.md` in the gyro skill's directory and follow it to write the plan. Surface to the user any decision it tells you to report, and stop if it changes approved scope. Preserve WIP and follow the user's git and publication instructions.
4. Run `gyro status <slug> --json` again; if the source changed or the ticket became stale, stop and identify the required review instead of presenting the plan as current.
5. Show the plan path and ask the user to review it. For a revised plan, also show which completed tasks stay valid and which were reopened, and wait for the user's review before suggesting execution resumes. Tell the user the next step is `/gyro-executing-plans <slug> <nn>` and stop. Do not invoke execution or change the sub-ticket status merely because a plan was written.

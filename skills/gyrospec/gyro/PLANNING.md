# Writing a sub-ticket plan

The rules for writing one sub-ticket's implementation plan. `/gyro-writing-plans` and the planner subagents of `/gyro-implement` both follow them; the calling skill owns the preflight, the revision case, and the handoff. This adapts Superpowers writing-plans; attribution is in this folder's `NOTICE.md`.

1. Read `.gyro/<slug>/tickets/NN-<name>.md`, the approved spec snapshot at the current revision, the approved PRD requirement blocks named in refs (map AC refs to their parent REQ), and relevant code and tests. Inspect the repository's instructions, glossary, ADRs, and tool commands. Compare the live `spec.md` to status; do not plan from an unapproved edit. An unresolved decision that changes approved scope stops planning: report it instead of deciding it.
2. Write `docs/superpowers/plans/<YYYY-MM-DD>-<slug>-<nn>.md`. Make the plan usable by an agent starting with no chat history: a capable engineer who writes idiomatic code once they know the exact interface and test. Planning is not building: write no source or test files, run no scratch builds, and do not implement any part of the slice to find out whether the plan works. Use this header, filled with actual values and local paths:

```markdown
# <Sub-ticket title> Implementation Plan

**Goal:** <Observable outcome>
**Architecture:** <Smallest design consistent with the approved spec>
**Tech Stack:** <Actual tools and versions in this codebase>
**Spec:** .gyro/<slug>/approved/spec.<n>.md
**Ticket:** <slug>
**Sub-ticket:** <nn>
**Spec revision:** <n>

## Global Constraints

<Invariants, approval boundaries, compatibility, and repo rules>

## Review Focus

<Specific failure modes and evidence a reviewer should inspect>

## Files

<Every file the plan creates, modifies, or tests, one per line>
```

3. Break the slice into tasks in dependency order, each one action with a checkable result. Each task names the REQ/AC IDs it serves and the exact files to create, modify, and test. Define consumed and produced interfaces where tasks meet, in an **Interfaces** block per task; a task that uses another task's output refers to that block instead of repeating it. Give concrete data, input/output examples, expected errors, and exact verification commands so the executor does not have to make product decisions. Keep all tasks within this sub-ticket's approved behavior; avoid unrelated improvements or speculative abstractions. When a task changes UI, give its loading, error, empty, and success states each a test or a verification step, name the input that is debounced and its delay, and make the final verification cover narrow and wide layouts, keyboard and screen-reader access, and both light and dark mode.

   A plan records decisions; it is not a transcript of the code. A test step names the test file, the test, and its assertions (inputs and expected results). A code step gives the file, the exact signature or interface, and the spec's values, and includes a body only for an algorithm those do not determine. A verification step gives the command and its expected passing output. Placeholders are the opposite failure: never write "add validation" or "handle errors" without saying which inputs and which results.
4. Give each task explicit TDD steps: write the named failing test, run its exact command and expect the stated failure, implement the smallest behavior, rerun and expect the stated success, then run the appropriate typecheck or project checks. Use checkboxes for progress. Include final integration verification and how it proves the referenced acceptance criteria. A plan is not authorization to commit, deploy, or open a PR.
5. Review the plan for complete acceptance coverage, correct paths, runnable commands, task boundaries, and consistency with the approved spec revision. Check its proportion: compare its length with the spec sections this sub-ticket implements. A plan several times longer than those sections, or one where code blocks dominate, is a transcript; replace the bodies with signatures and test assertions.

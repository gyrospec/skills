---
name: gyro-retro
description: Review how a finished or blocked sub-ticket actually went, trace each surprise to the phase that should have caught it, and propose fixes.
disable-model-invocation: true
---

# Retro

1. Call the Skill tool with "gyro". Follow its Node 20+ check, script path, and language rules. Reply in the user's language and keep convention keywords English.
2. Require `<slug> <nn>`, normalize `nn`, and run `gyro status <slug> --json`. The sub-ticket must exist and be `done` or `blocked`; a `todo` or `in_progress` ticket has nothing to learn from yet, so show its status and stop.
3. Gather the evidence, and keep a `path:line` or commit for every item you will cite:
   - the sub-ticket file, its plan (checked tasks and their evidence, deviations, `**Out of scope:**`, `## Review findings`), and the run ledger `docs/superpowers/plans/<slug>-implement.md` when `/gyro-implement` ran it;
   - the approved spec sections it implements, the PRD blocks in its refs, and the discovery entries behind them;
   - `git log` and `git diff` from the plan's `**Execution start:**` to the ticket's merge or PR head;
   - the local session transcripts for this repository, when the harness keeps them on disk (Claude Code under `~/.claude/projects/`, Codex under `~/.codex/sessions/`). Tell the user which ones you will read. Transcripts are long, so start one subagent with a fresh context window, never a fork, to read them and report what went wrong with `path:line` evidence: repeated work, an ignored plan step, a check run late or not at all, a fix loop, a question the user had to answer twice.
4. Find each place where reality differed from the plan, the spec, or the PRD: a deviation, a blocker, a review finding, a fix loop, rework, a test added late, or a user correction. For each one, name the phase that should have caught it and how:
   - discovery: a question never asked or a dimension skipped;
   - PRD: an AC that could not be checked or a missing negative scenario;
   - prototype or spec: a flow, interface, or error left open;
   - breakdown: a ticket too big for one context window or a wrong blocking edge;
   - plan: a wrong path, a missing case, or a plan that transcribed code;
   - execution: a skipped check or an ignored cap;
   - review: a finding that an automated check could have caught first.

   Separate what the evidence shows from your interpretation of it. Never quote secrets, credentials, or personal data from transcripts; cite the location instead.
5. Write `.gyro/<slug>/retros/<nn>.md` with sections `Summary`, `What happened` (each item with its evidence), `Root causes` (each with the phase that should have caught it), and `Proposals`.
6. Propose fixes, numbered, each with its target and a recommendation:
   - this repository's instructions (`AGENTS.md`, `CLAUDE.md`), glossary, or a new ADR;
   - a new automated check (test, lint rule, or type) that would catch the problem next time, preferred over a new instruction whenever one exists;
   - a follow-up sub-ticket, through `/gyro-breakdown <slug>`;
   - feedback on Gyrospec itself: draft the issue text for `gyrospec/skills` for the user to file, and file nothing yourself.

   Apply only the proposals the user accepts, and record each answer in the retro file. Never edit an approved artifact or its snapshot; a change to the PRD, prototype, or spec goes through its own phase and approval.
7. Run `gyro status <slug> --json`. Report the retro path and what was applied, tell the user the next step is the returned `next`, and stop. Do not invoke it.
